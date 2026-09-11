# ADR-0033: Statute verification is due by elapsed build day 21 (FB-01)

| | |
|---|---|
| **Purpose** | Gives the verification of statutory claims a deadline that can actually be reached. |
| **Intended reader** | The owner, and whoever runs the final v1 eval. |
| **Doc status** | In Review |
| **Decision status** | Accepted, 2026-09-12 (step 4) |
| **Resolves** | The owner's finding FB-01 (2026-09-11) |
| **Amends** | The feasibility review §7, row O-03 |
| **Related** | PRD Appendix A, Appendix B; `06` Appendix A; ADR-0005 |

## Context

The feasibility review set O-03, the verification of V01 to V24, as due "before any statutory output leaves the developer's machine". Nothing leaves the machine in v1. So that condition never fires, and the 24 tags would stay unverified indefinitely.

## Decision

- **V01 to V24 are verified by elapsed build day 21,** alongside O-04, the loading of the verified statute text. That way the final eval run exercises the L4 notice against verified provisions.
- **If a tag is still unverified at day 21,** the eval report lists it, and every statutory artifact that depends on it stays labelled unverified.
- **V25 to V43**, the data-protection and security tags in `06` Appendix A, bind at the pilot. They are due before the pilot gate (`12` §9.1).

## Alternatives rejected

| Alternative | Why rejected |
|---|---|
| Keep the old trigger | It never fires |
| Verify only at the pilot gate | The final v1 eval would run on unverified provisions |

## Consequences

- PROJECT_CONTEXT O-03 is split: V01 to V24 are due by build day 21, and V25 to V43 before the pilot gate.
- The feasibility review §7 is revised to match.
