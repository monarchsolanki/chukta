# ADR-0019: Repository conventions

| | |
|---|---|
| **Purpose** | Records how this repository is kept: where it lives, how commits are made, and how docs are versioned. |
| **Intended reader** | Anyone committing to this repository, including a future session. |
| **Doc status** | In Review |
| **Decision status** | Accepted, 2026-09-11 (checkpoint 1) |
| **Related** | BRIEF §4 rules 6 and 7; `PROJECT_CONTEXT.md` |

## Context

The brief requires a single `main` branch, conventional commits, no `Co-Authored-By` trailers, one commit per deliverable, and a standard header on every doc. On 2026-09-11 the owner also moved the repo out of `~/N073`, asked for a master document, and ruled that no commit in any project may name Claude as a collaborator.

## Decision

- **Location:** `~/chukta`, never inside `~/N073`, which pushes to a company org. Remote: `monarchsolanki/chukta`, private at first and public since 2026-09-13 at the owner's request. Anything committed is published.
- **Identity:** commits use the owner's GitHub noreply address, set repo-locally. They never use the college address.
- **Commits:** single `main` and conventional messages. **No `Co-Authored-By` trailer and no Claude attribution of any kind.** That is the owner's standing rule for every project. One commit per completed deliverable. The ADR set counts as one deliverable.
- **Doc headers:** every doc opens with purpose, intended reader and status (Draft, In Review or Frozen). ADRs also carry a decision status (Proposed, Accepted or Superseded). A superseded ADR is never edited. A new ADR supersedes it, and the two link to each other.
- **Brief:** `BRIEF.md` is verbatim and Frozen. Corrections go in ADRs and PRD Appendix C.
- **Master doc:** `PROJECT_CONTEXT.md` is updated with every deliverable. Doc defects go in its bug register, with symptom and root cause kept separate.

## Alternatives rejected

| Alternative | Why rejected |
|---|---|
| One commit per file | A noisy history that hides which deliverable a change belongs to |
| Keep the repo inside N073 with a local exclude | Rejected by the owner as an accident risk |
| Commit with the global git identity | That is the college address, which the owner ruled out |

## Consequences

- A status change needs an edit and a log entry.
- Before every push: check that no commit message contains a `Co-Authored-By` trailer or any Claude attribution.
