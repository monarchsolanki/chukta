# ADR-0015: Langfuse Cloud for v1, with trace masking

| | |
|---|---|
| **Purpose** | Records where traces go in v1, and what must change before any real data exists. |
| **Intended reader** | Anyone working on observability, evals, cost reporting or data protection. |
| **Doc status** | In Review |
| **Decision status** | Accepted, 2026-09-11 (checkpoint 1) |
| **Related** | PRD NFR-03, NFR-10, SM-05; ADR-0016 |

## Context

The brief asks for Langfuse tracing and per-case token cost. Self-hosting Langfuse v3 needs ClickHouse, Redis and S3-compatible storage alongside Postgres. That would be the heaviest infrastructure in the stack. Traces contain prompt content, and once real data exists, prompts will carry counterparty personal data.

## Decision

- Use **Langfuse Cloud** for v1, which runs on synthetic data only.
- Design masking now. A mask function runs on every trace before it leaves, through the SDK's masking hook. It pseudonymises personal identifiers the same way hosted-model calls do (NFR-03).
- Trace metadata carries tenant, case, node, model, tokens and cost.
- **Real-data gate:** while this ADR is in force, a config flag prevents creating a non-synthetic tenant. Before any real data, a new ADR re-decides: either Cloud with masking, a data processing agreement and a suitable region, or self-hosting.

## Alternatives rejected

| Alternative | Why rejected |
|---|---|
| Self-host v3 now | The heaviest infrastructure for one developer, with no benefit on synthetic data |
| No tracing, only log files | Loses the per-case cost and trajectory inspection that the eval plan needs |
| Prometheus only | Gives metrics, not traces |

## Consequences

- Trace data leaves our infrastructure. That is acceptable only while data is synthetic.
- SM-05 covers trace payloads as well as hosted-model payloads.
- The real-data gate is tested: creating a real tenant fails while this ADR is in force.
