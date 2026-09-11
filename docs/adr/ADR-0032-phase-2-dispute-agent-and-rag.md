# ADR-0032: Phase 2: the dispute agent and the RAG layer that serves it, bounded to 8 days

| | |
|---|---|
| **Purpose** | Records why RAG and the dispute agent are deferred from v1 but not dropped, and the hard bounds on when and how they return. |
| **Intended reader** | The owner, Hat B (who writes the Phase 2 paragraph in `09`), and whoever designs `04` and `05`. |
| **Doc status** | In Review |
| **Decision status** | Accepted, 2026-09-12 (step 4) |
| **Resolves** | The owner's decision OD-1, as corrected on 2026-09-11. The scheduling of DF-09 and DF-11. SM-18's return for dispute findings (the owner's direction, 2026-09-12). |
| **Related** | `12` §9.2; ADR-0021, ADR-0024, ADR-0031; feasibility review §5.2 |

## Context

The feasibility review deferred the RAG layer, because once the dispute agent is deferred, nothing in v1 retrieves from a corpus. The owner agreed for v1 but corrected the plan's shape. RAG and the dispute agent are the two components that demonstrate retrieval engineering. The 28-day box was arbitrary. Deferring them is right for v1 and wrong permanently. Bringing RAG back together with its consumer answers the review's objection instead of overriding it.

## Decision

- **v1 is unchanged:** 24 days, the committed 22, the build-if-time order and the day-12 rule.
- **Phase 2 is a named follow-on of 8 working days, with a hard stop.** It starts after v1 closes, and it is not an open backlog.
- **Its only feature is the dispute agent (DF-09), together with the RAG layer that serves it (DF-11):**
  - hybrid retrieval: Postgres full-text search, pgvector and a reranker
  - graph retrieval over the entity links that v1 already stores, using recursive CTEs
  - multi-hop retrieval for dispute investigation
  - the dispute agent itself (FR-DSP-1), plus the manual acceptance-reset action (FR-DSP-2) if v1 did not build it
- **What Phase 2 measures:**
  - SM-17, retrieval quality
  - SM-19, dispute assessment agreement
  - SM-20, dispute evidence recall, measured by the trajectory suite
  - **SM-18, citation faithfulness, scoped to dispute findings only.** FR-DSP-1 requires every finding to cite a span, and a model chooses those spans, so whether they hold up must be measured. Statutory artifacts stay template-only, and G6 stays skipped for them (ADR-0024).
- **The trajectory suite lands with the feature,** per the feasibility review's §6 rule, not as hardening at the end.
- **The dispute agent sends retrieved spans to the frontier model.** So DF-14's name-finding pseudonymisation pass must land in Phase 2, before the first hosted dispute call (ADR-0021). If it slips, the dispute agent runs on the local SLM until it lands.
- **There is no slack.** The feasibility review's estimates for the parts sum to exactly 8 days: 4 for RAG, 3 for the dispute agent with its trajectory evals, and 1 for the name-finding pass.
- **Hard stop:** at day 8, whatever is unfinished goes back to `12` §9.2 as a decision for the pilot gate. Phase 2 does not extend.
- **DF-09 and DF-11 stay in `12` §9.2 as they are.** Phase 2 is how they get resolved, not a change to the gate.
- `09-BUILD-PLAN.md` gets one paragraph naming Phase 2, its 8 days and its hard stop (Hat B, step 5).

## Alternatives rejected

| Alternative | Why rejected |
|---|---|
| Defer both to the pilot gate with no schedule | Right for v1, but wrong permanently. The project would never show retrieval engineering. |
| Build RAG in v1 without its consumer | It would be built only to feed its own metric (the feasibility review's objection) |
| An open-ended Phase 2 backlog | It would become the place scope goes to grow |

## Consequences

- `04-RAG-DESIGN.md` designs for Phase 2, not v1.
- `05-EVAL-PLAN.md` places SM-17 to SM-20 in Phase 2.
- The v1 data model stores entity links from the start, so Phase 2 needs no migration of existing data.
- SK-01 narrows to the G6 check on statutory prose (ADR-0024).
