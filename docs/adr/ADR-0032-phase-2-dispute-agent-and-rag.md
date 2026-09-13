# ADR-0032: Phase 2: the dispute agent and the RAG layer that serves it, bounded to 8 days

| | |
|---|---|
| **Purpose** | Records why RAG and the dispute agent are deferred from v1 but not dropped, and the hard bounds on when and how they return. |
| **Intended reader** | The owner, Hat B (who writes the Phase 2 paragraph in `09`), and whoever designs `04` and `05`. |
| **Doc status** | In Review |
| **Decision status** | Accepted, 2026-09-12 (step 4). Metric set decided by the owner, DLT-04 and the `04` move amended, 2026-09-13. |
| **Resolves** | The owner's decision OD-1, as corrected on 2026-09-11. The scheduling of DF-09 and DF-11. SM-18's return for dispute findings, decided by the owner on 2026-09-13 (O-17). DLT-04. The move of `04-RAG-DESIGN.md` to Phase 2. |
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
- **The owner's decision on the metric set** (O-17, 2026-09-13), in the owner's words: "Phase 2 measures SM-17 (retrieval quality), SM-19 (dispute agreement) and SM-20 (dispute evidence recall), plus SM-18 (citation faithfulness) scoped to dispute findings only, not to statutory artifacts. My earlier 'SM-16, SM-17, SM-19' used pre-DOC-01 numbers. It was wrong and SM-16 stays out: document classification belongs to OCR, deferred as DF-06."
- **Phase 2's security suites live inside the 8 days** (DLT-04, amended 2026-09-13). They are extensions of ST-01 (stored-injection cases against the dispute agent, on a real model) and ST-03 (the retrieval API), not new suites. Budget: 0.5 day.
- **Phase 2's version of the day-12 rule:** when Phase 2 runs short, the dispute agent's features shrink, never its suites. With the 0.5 day of suites, the estimates above sum to 8.5 days, so Phase 2 starts half a day short and the rule applies from day one.
- **`04-RAG-DESIGN.md` is written on Phase 2's first day,** inside the RAG estimate, not in step 5a (amended 2026-09-13). Retrieval is deferred from v1 entirely, and a design written now would be rewritten once the dispute agent's real needs appear (ADR-0035).

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
