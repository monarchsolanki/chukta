# ADR-0034: SM-21 is always reported with its cause (FB-02)

| | |
|---|---|
| **Purpose** | Stops the local-model share from being read as an optimisation result when it is a side effect of scope. |
| **Intended reader** | Whoever writes eval reports, and anyone reading them. |
| **Doc status** | In Review |
| **Decision status** | Accepted, 2026-09-12 (step 4) |
| **Resolves** | The owner's finding FB-02 (2026-09-11) |
| **Amends** | The PRD's SM-21 row. It also adds to ADR-0017's reporting rules. |
| **Related** | ADR-0016, ADR-0021; DF-14 |

## Context

The feasibility review noted that SM-21, the local SLM share with a target of at least 80%, becomes easier to meet in v1, because hosted calls are limited to drafting. Without that context, a high figure reads like the result of routing optimisation.

## Decision

- **The PRD's SM-21 row records the cause.** In v1, a high local share is partly a consequence of deferring the free-text hosted path (DF-14). It is not evidence of routing optimisation.
- **Every eval report that shows SM-21** states this next to the figure, together with the list of task types that were eligible for hosted calls in that run.
- **When DF-14 lands,** in Phase 2 or later, SM-21 is reported separately for the new mix of tasks. It is never compared with v1 figures without a note.

## Alternatives rejected

| Alternative | Why rejected |
|---|---|
| Report the bare number | It misleads |
| Drop SM-21 in v1 | The brief asks for the routing split |

## Consequences

- The eval report template in `05` carries a field for this caveat.
- ADR-0017's rule that results are reported per data source gains a matching rule for SM-21: it is reported per routing mix.
