# ADR-0013: Celery for compute, `wake_at` rows in Postgres for timers, and an outbox

| | |
|---|---|
| **Purpose** | Records the job system choice the brief asked for, and how multi-day waits and cross-store commits are handled. |
| **Intended reader** | Anyone working on workers, case resumption, timers or idempotency. |
| **Doc status** | In Review |
| **Decision status** | Accepted, 2026-09-11 (checkpoint 1). Modified: the Postgres-only queue is rejected, with the reason recorded below. |
| **Related** | PRD §5, §5.9, FR-CASE-2, NFR-07, NFR-08, SM-06; ADR-0009, ADR-0014 |

## Context

The brief asks us to pick BullMQ (Node) or Celery (Python) and justify the choice. The async work is Python: OCR, parsing, model calls and graph resumes. Cases wait for days. With a Redis broker, a Celery ETA or countdown task that outlives the visibility timeout gets redelivered. And a case state change in Postgres and its job enqueue in Redis cannot commit atomically.

## Decision

- **Celery with Redis** runs the compute tasks. Node enqueues through FastAPI endpoints, never directly.
- **Multi-day waits are `wake_at` rows in Postgres.** A Celery beat task sweeps due rows every minute with `FOR UPDATE SKIP LOCKED` and enqueues the resumes.
- **Outbox:** any state change that needs follow-up work writes an outbox row in the same transaction. A relay publishes outbox rows to Celery and marks them published.
- Consumers are idempotent. Each task carries a deduplication key.

## Alternatives rejected

| Alternative | Why rejected |
|---|---|
| BullMQ | The work is Python. It would need the Python port of BullMQ or a Node shim. |
| A Postgres-only queue (a `SKIP LOCKED` table, or a library such as Procrastinate) | It would mean hand-rolling a worker pool for the compute path: concurrency, retries, time limits and prefetch, all of which Celery already provides. More work, not less. |
| Celery ETA for multi-day waits | Unreliable with the Redis visibility timeout |

## Consequences

- Redis is transport only. Postgres is the source of truth.
- Delivery is at-least-once, so every handler must be idempotent (SM-06).
- Two extra processes run: the beat sweeper and the outbox relay.
- Each case needs a concurrency lock so that only one run is active at a time (ADR-0009).
