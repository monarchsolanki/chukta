# ADR-0022: Durable execution details

| | |
|---|---|
| **Purpose** | Records the clock, locking and outbox-relay details behind durable cases. |
| **Intended reader** | Anyone working on the case engine, timers or workers. |
| **Doc status** | In Review |
| **Decision status** | Accepted, 2026-09-12 (step 4) |
| **Resolves** | `01` D-05 and D-07, and the feasibility review §8 change to ADR-0013 |
| **Amends** | ADR-0013 |
| **Related** | `01` §6; ADR-0009 |

## Context

`01` §6 set the clock and locking rules for case runs. The feasibility review then moved the outbox relay into the scheduler for v1. None of this was recorded in an ADR.

## Decision

- **Every due-time comparison uses Postgres `now()`,** never a host clock (D-05).
- **Case runs are serialised by a transaction-scoped Postgres advisory lock plus a dirty flag.** A trigger that arrives mid-run makes the running worker loop once more before it exits (D-07).
- **In v1, the outbox relay runs inside the scheduler process.** The outbox design in ADR-0013 is unchanged. Only the process that carries the relay is different.

## Alternatives rejected

| Alternative | Why rejected |
|---|---|
| Host clocks | Clock skew between hosts can fire a timer early or miss it |
| A lock without a dirty flag | Loses a trigger that arrives mid-run |
| A queue per case | More moving parts for the same guarantee |
| A separate relay process in v1 | One more process to run and watch, with no gain at v1 volume |

## Consequences

- The exactly-once tests (SM-06) cover the relay running inside the scheduler.
- The production target may split the relay into its own process without a new decision.
