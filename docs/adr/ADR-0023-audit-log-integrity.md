# ADR-0023: Audit log integrity

| | |
|---|---|
| **Purpose** | Records how the audit log resists tampering, and what v1 builds of it. |
| **Intended reader** | Anyone working on the audit table, database roles or tamper tests. |
| **Doc status** | In Review |
| **Decision status** | Accepted, 2026-09-12 (step 4) |
| **Resolves** | `01` D-08, `06` S-10, SR-10, and the feasibility review's build-if-time item 4 (fallback DF-20) |
| **Related** | `01` §7, §9; ADR-0027 |

## Context

`01` D-08 made audit events append-only and hash-chained. Hat C then pointed out (SR-10) that a chain held inside the database can be recomputed by anyone who can write to it. The feasibility review put the chain's anchoring into build-if-time.

## Decision

- **In v1 core,** audit events are append-only, and the runtime role has INSERT only on the audit table (ADR-0027).
- **Build-if-time item 4:** the hash chain, a daily anchor of the chain head outside the database (an append-only file included in backups in v1, and an S3 bucket with Object Lock in the target), a nightly verification job, and ST-12.
- **If item 4 is not built by the end of v1,** it becomes DF-20: required before the pilot.

## Alternatives rejected

| Alternative | Why rejected |
|---|---|
| A plain table | Edits could not be detected at all |
| Anchoring in v1 core | 0.75 of a day, against a threat that barely exists on one machine holding synthetic data |

## Consequences

- Until item 4 is built, the INSERT-only grant stops the application from editing audit rows. It does not stop someone with owner access to the database. That is accepted for v1.
- ST-12 lands with item 4.
