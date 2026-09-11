# ADR-0035: Hat C ran before Hat A's 02 to 05, with a second delta pass

| | |
|---|---|
| **Purpose** | Records the owner's change to the review sequence, which until now lived only in the master doc. |
| **Intended reader** | Anyone running the remaining steps, and whoever reads SEC-REVIEW-ARCH later. |
| **Doc status** | In Review |
| **Decision status** | Accepted, 2026-09-12 (step 4). It records the owner's instruction of 2026-09-11. |
| **Resolves** | The owner's sequence change (PROJECT_CONTEXT O-11) |
| **Amends** | ADR-0018 |
| **Related** | BRIEF §3; `reviews/SEC-REVIEW-ARCH.md` |

## Context

ADR-0018 followed the brief: Hat A writes `00` to `05`, and then Hat C reviews them. On 2026-09-11 the owner moved Hat C ahead of Hat A's `02` to `05`. So Hat C reviewed only the PRD, `01` and the ADRs.

## Decision

- **The sequence as it has been run and planned:**
  1. Hat A: `00`, `01` and the ADRs
  2. Hat C: `06`, `12` and SEC-REVIEW-ARCH
  3. Hat B: the feasibility review
  4. Hat A: the step 4 revision
  5. Hat C: delta 1
  6. Hat A: `02` to `05`
  7. Hat C: delta 2, over `02` to `05`
  8. Hat B: `07` to `11`
  9. README and INDEX
- **Delta 2 (O-11)** reviews `02` to `05` once they are written, and is appended to SEC-REVIEW-ARCH. The owner decides whether it gets its own STOP.

## Alternatives rejected

| Alternative | Why rejected |
|---|---|
| Wait for `02` to `05` before any security review | Not what the owner chose. It would also have meant SR-01 to SR-04 reached `02` to `05` only after they were written, instead of shaping them. |

## Consequences

- `02` must carry out SEC-01 and SR-04 (ADR-0027).
- `03` and `04` must carry out SR-06 (ADR-0029).
- Delta 2 checks that they did.
