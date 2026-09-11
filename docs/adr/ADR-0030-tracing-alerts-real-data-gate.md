# ADR-0030: Tracing, alerts and the real-data gate in v1

| | |
|---|---|
| **Purpose** | Records what v1 uses for cost and observability, and how the synthetic-only rule is enforced rather than remembered. |
| **Intended reader** | Anyone working on the spend ledger, tracing, alerts, tenant creation or ST-14. |
| **Doc status** | In Review |
| **Decision status** | Accepted, 2026-09-12 (step 4) |
| **Resolves** | `06` S-11; SR-14; the feasibility review §8 changes to ADR-0015, `01` §11 and NFR-10; build-if-time items 3 (fallback DF-19) and 5 (fallback SK-06); DF-05 |
| **Amends** | ADR-0015 |
| **Related** | `12` §8, §9; ADR-0021, ADR-0031 |

## Context

ADR-0015 chose Langfuse Cloud with masking, and a real-data gate held by a flag. Hat C found that the gate relied on the flag alone (SR-14). The feasibility review then moved Langfuse to build-if-time and deferred Prometheus.

## Decision

- **The spend ledger in Postgres is the source of truth** for cost and model-call counts in v1. Cost per case (SM-22) comes from it.
- **Langfuse tracing with masking is build-if-time item 3.** Until it exists, NFR-10's full trace coverage is not met, and every eval report says so. If it is not built by the end of v1, it becomes DF-19 (Required, `12` §9.1 condition 11). When it is built, traces are written from pseudonymised payloads (ADR-0021).
- **Prometheus metrics and alerts,** including the queue-ageing alerts, are DF-05, Required before the pilot. In v1, queue ages appear as flags in the Console (FR-APR-3 and FR-HQ-5, ADR-0031).
- **The real-data gate is enforced by the data model, not by memory.** Every tenant row carries `synthetic = true`, and a check constraint rejects any other value while this ADR is in force (B2).
- **Detection of real-looking identifiers at ingestion,** with ST-14, is build-if-time item 5. If it is not built, it becomes SK-06, because it is moot once the pilot gate lifts.
- **Lifting the gate** needs every condition in `12` §9.1, every Required row in §9.2, and a new ADR.

## Alternatives rejected

| Alternative | Why rejected |
|---|---|
| Langfuse in v1 core | Half a day for inspection that the spend ledger mostly covers |
| Prometheus in v1 | Alerts on a single developer machine alert nobody |
| A flag alone for the real-data gate | Relies on people remembering |

## Consequences

- Eval reports state their trace coverage.
- PRD NFR-10 is revised to apply "once tracing is built".
- `01` §11 is revised.
