# ADR-0040: The drafting node must earn its hosted path: SM-25 and a blind comparison (FB-03)

| | |
|---|---|
| **Purpose** | Applies the load-bearing-AI test to the last component it had not been applied to: N-11 `draft_prose`, the only node that can reach a hosted model. It adds the measurement, and the rule for what happens if the node earns nothing. |
| **Intended reader** | Whoever writes `05` and runs the eval phase. Anyone deciding whether v1 runs hosted or local-only. |
| **Doc status** | In Review |
| **Decision status** | Accepted, 2026-09-13. Records the owner's finding FB-03. |
| **Resolves** | FB-03 (2026-09-13) |
| **Amends** | PRD §6.2 and §10.2 (adds SM-25); `02` §6.8 and §6.9; `03` §4.1 and §6 |
| **Related** | PRD §6.1; ADR-0007, ADR-0021, ADR-0034, ADR-0036, ADR-0037 |

## Context

Three facts together, as the owner put them:
1. N-11 is the sole hosted path.
2. It silently falls back to the template's fixed prose when its output fails the schema.
3. No system metric measures its output.

Meanwhile about 0.75 day of C-1's 1.5 days, plus ADR-0037's entire privacy analysis, exist to serve that one node. The project applied the load-bearing-AI test (PRD §6.1) to every other component, and never to this one. If N-11 falls back on most runs, or its prose is no better than the template, v1 carries a hosted path, a free-tier constraint and a quota subsystem for nothing measurable.

## Decision

- **SM-25, drafting fallback rate.** It measures the share of finalised non-statutory artifacts that shipped with the template's fixed prose instead of model prose.
  - It is reported **per mode** (local-only and hosted) and never blended.
  - Statutory artifacts skip N-11 by design (ADR-0024), so they are excluded from the count.
  - **It costs nothing to collect:** every N-11 run already writes a `spend_ledger` row. That row now records an `outcome` (`02` §6.9), and each artifact version records its `prose_source` (`02` §6.8).
- **A blind drafting comparison, run in the eval phase and specified in `05`:**
  - 20 artifacts are rendered two ways: with model prose, and template-only.
  - Three people each pick which one they would rather send, without knowing which is which.
  - It is reported with its n and its interval, like the H set (ADR-0017).
  - It costs an afternoon during the eval phase, not a build day.
- **The decision rule is written before the comparison runs.** `05` defines what "no difference" means, and the threshold is set before any rater sees an artifact, so the result cannot be interpreted after the fact.
- **If the comparison shows no difference, that is a finding worth having.** v1 then runs in local-only mode. The hosted path, ADR-0036's quota handling and ADR-0037's terms stay specified but unused, and the eval report says so.
- **If SM-25 shows most runs falling back,** the same rule applies: the prose is not reaching the reader, so the hosted path has not earned its place.

## Alternatives rejected

| Alternative | Why rejected |
|---|---|
| Keep N-11 unmeasured | It is the one AI component whose value is only assumed |
| An LLM judge instead of people | A model judging model prose is the circularity ADR-0017 exists to avoid |
| Remove the hosted path now | That would be deciding before measuring. The comparison is cheap. |
| Interpret the comparison after seeing the results | Invites reading a null result as a win |

## Consequences

- PRD §10.2 gains SM-25, at the end of its table so the IDs stay in reading order.
- `02` gains `spend_ledger.outcome` and `artifact_version.prose_source`.
- `03` N-11 records both.
- `05` specifies the comparison, its interval method and its pre-set rule. Whether to add local SLM prose as a third arm is decided there (D3-Q4).
