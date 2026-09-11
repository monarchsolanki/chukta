# ADR-0018: Hat C writes 06 and 12 at step 2, with a delta pass after step 4

| | |
|---|---|
| **Purpose** | Records how the three-hat review sequence handles the security docs the brief left unplaced. |
| **Intended reader** | Anyone running the next steps of the documentation sequence. |
| **Doc status** | In Review |
| **Decision status** | Accepted, 2026-09-11 (checkpoint 1) |
| **Related** | BRIEF §3; PROJECT_CONTEXT checkpoint tracker |

## Context

The brief's sequence runs: Hat A, Hat C review, Hat B review, Hat A revision, Hat B deliverables, then the final README and index, with a STOP after each. It does not say when Hat C writes `06-SECURITY-THREAT-MODEL.md` and `12-DATA-CLASSIFICATION.md`. Step 4 also changes the architecture after the threat model already exists.

## Decision

- Hat C writes `06`, `12` and `reviews/SEC-REVIEW-ARCH.md` at step 2.
- After step 4, a short Hat C delta pass reviews the revised docs and appends its findings to `SEC-REVIEW-ARCH.md`. It adds no extra STOP.
- Checkpoints stay as the brief sets them. Only the owner can waive one, explicitly. On 2026-09-11 the owner asked for a STOP after the PRD fixes and ADRs, before `01-ARCHITECTURE.md`.

## Alternatives rejected

| Alternative | Why rejected |
|---|---|
| Hat C writes `06` after step 4 | The step 2 review would have no threat model to review against |
| A full Hat C re-run after step 4 | Duplicate work. A delta pass covers what changed. |

## Consequences

- `SEC-REVIEW-ARCH.md` has two sections: the initial review and the delta.
- The tracker in `PROJECT_CONTEXT.md` shows the delta as part of step 4.
