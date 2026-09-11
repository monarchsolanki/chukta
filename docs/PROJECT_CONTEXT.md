# Chukta: Project Context (master document)

| | |
|---|---|
| **Purpose** | The single master record for Chukta: current state, what is open, every decision and why, every bug and its cause, and what comes next. |
| **Intended reader** | Anyone picking the project up, including a future session with no memory of this one. Read Part 1 first. |
| **Status** | Draft. This file is never frozen. It is updated in the same commit as every deliverable. |
| **Last updated** | 2026-09-11 |

**Two parts, and they age in opposite directions.**

| Part | Contents | Rule |
|---|---|---|
| **Part 1: Now** | START HERE, what is open, traps, how to keep this file current, document map | **Rewrite** whenever anything changes |
| **Part 2: Record** | Dated log, decision register, bug register, plans, lessons | **Append only.** Fix a wrong entry by adding a dated correction under it. Never edit history silently. |

---

# Part 1: Now

## START HERE (one screen, verified 2026-09-11)

- **Phase:** design documentation only. No application code until the doc set is complete ([BRIEF](BRIEF.md), "Your task in this session").
- **Current step:** `01-ARCHITECTURE.md` is approved, with the owner's findings F-01 to F-05 applied, and frozen for step 2 review. **Hat C (step 2) is in progress:** `06`, `12` and `reviews/SEC-REVIEW-ARCH.md`.
- **Sequence change (owner, 2026-09-11):** Hat C runs now, ahead of Hat A's `02` to `05`. Hat C reviews the PRD, `01` and the ADRs. `02` to `05` get a Hat C delta pass when they are written (O-11).
- **🛑 STOP after Hat C's three deliverables** (brief step 2).
- **Everything is committed and pushed.** Run `git -C ~/chukta log --oneline` to confirm.

| | Verified 2026-09-11 |
|---|---|
| Repo | `~/chukta`, a sibling of `~/N073` and **not inside it**. Branch `main`, pushed to private GitHub repo `monarchsolanki/chukta`. Commits use the owner's GitHub noreply address, set repo-locally, and never credit Claude. |
| Commits | `947cc5a` brief · `e50d320` PRD as approved · `75cc43c` ADR-0001 to 0019 · `078b7d4` PRD fixes · `a9d8893` this file · `e212bdd` `01-ARCHITECTURE.md` · then the F-01 to F-05 fixes |
| Written | `BRIEF.md` (Frozen), `PROJECT_CONTEXT.md` (this file), `00-PRD.md` (Frozen for step 2 review), `adr/ADR-0001` to `ADR-0019` (In Review, all Accepted), `01-ARCHITECTURE.md` (Frozen for step 2 review) |
| Pending | Hat C now: `06`, `12`, SEC-REVIEW-ARCH. Then Hat A's 02 to 05, and ADRs for `01`'s D-01 to D-09 (O-08). IMPL-FEASIBILITY-REVIEW, 07 to 11 (Hat B). README, INDEX. |
| Statutory corpus | Empty by design. The owner loads verified text in build Phase 4 (PRD Appendix B). |
| Running system | None. v1 is not built. |

**Checkpoint tracker** (sequence from BRIEF §3)

| Step | Hat | Deliverables | Status |
|---|---|---|---|
| 0 | A | Understanding, 19 recommendations, PRD TOC | ✅ Done 2026-09-11. All 19 adopted, six with modifications. |
| 1 | A | 00 to 05, ADRs | 🟡 00-PRD and 01-ARCHITECTURE frozen for review. ADR-0001 to 0019 written. 02 to 05 not started: the owner moved Hat C ahead of them. |
| 2 | C | 06, 12, SEC-REVIEW-ARCH | 🟡 In progress. Reviews 00, 01 and the ADRs. |
| 3 | B | IMPL-FEASIBILITY-REVIEW | Not started |
| 4 | A, then a short C delta pass | Revisions, logged in ADRs | Not started |
| 5 | B | 07 to 11 | Not started |
| 6 | Final | README, INDEX | Not started |

## What is actually open (verified 2026-09-11)

O-01 (commit identity) and O-02 (remote) closed on 2026-09-11. See the log.

| ID | Item | Owner | Blocks |
|---|---|---|---|
| O-03 | Verify every tag in PRD Appendix A against primary sources | Monarch | Statutory design freeze |
| O-04 | Load statute text per PRD Appendix B | Monarch | Build Phase 4 |
| O-05 | Hand-write 80 Hinglish messages, with 2 or 3 other contributors | Monarch | Conversation eval |
| O-06 | Confirm the `qwen3.6:27b` Ollama tag exists and fits the developer's machine (v1 has no GPU host, `01` A-Q5). If it does not fit, pick a smaller local model. Name the default frontier model. | Monarch, then ADR-0016 | Model routing design |
| O-07 | Default CA-review policy for formal notices. Proposed: required by default, and the owner can waive it with an audit event. | Monarch | PRD §7.2 |
| O-08 | Promote the `01-ARCHITECTURE.md` decisions D-01 to D-09 to ADRs when Hat A's set closes. Until then, §15 of `01` is their only record. | Hat A | Step 1 close |
| O-09 | Answer the architecture open questions A-Q1 to A-Q8 (`01` §16): mail provider and region, v1 runtime confirmation, egress mechanism in the target, auth and MFA, GPU affordability, OCR engine, region, and MCP versus P9 | Hats A, B and C, as listed in `01` §16 | Steps 2 to 5 |
| O-10 | The PRD has no requirement for approval-queue ageing. F-04 added only a signal and an alert in `01` §11. Add a requirement through an ADR at step 4. | Hat A | Step 4 |
| O-11 | Hat C delta pass over `02` to `05` once they are written, because the owner moved Hat C ahead of them | Hat C | After `05` |

## Traps: easy to get wrong

- **Never recreate the repo inside `~/N073`.** N073 pushes to the SETU company org. Chukta was nested there for a few minutes on 11 Sep and was moved out.
- **Never `git add -A` in `~/chukta`.** macOS drops `.DS_Store` files, and the repo has no `.gitignore`, because the brief allows only `docs/` and `README.md`. Add paths explicitly.
- **No Claude attribution on any commit or PR, in any project.** This is the owner's standing rule. Claude Code's global `attribution` setting in `~/.claude/settings.json` is set to empty strings. Check new commit messages before every push.
- **`BRIEF.md` is verbatim and frozen.** Corrections go in ADRs and PRD Appendix C, never into the brief.
- **"Three-way match" is the buyer's PO/GRN/invoice check.** Our matching is "ledger reconciliation".
- **The 43B(h) rule is a year-end deferral, not a loss.** Never write "lost" (PRD §5.8).
- **No statute from memory.** Every section, rate or deadline is tagged `[VERIFY Vnn: ...]` and listed in PRD Appendix A. The inline set and the Appendix A set must match exactly (DOC-03).
- **SM IDs run in reading order.** A new metric goes at the end of its block, or the whole table is renumbered by script and every reference updated (DOC-01).
- **No em dashes in the docs.**
- **Prisma must not manage LangGraph's tables** (ADR-0014).
- **v1 has no GPU host and no AWS deployment** (F-03). It runs on `docker compose up` on a developer machine. The AWS topology in `01` §10.3 is a specified target, not something to build.

## Keeping this current is part of the job

A wrong line here is worse than a missing one. It reads as authoritative and nobody re-checks it.

1. **Every commit that completes a deliverable updates this file in the same commit:** START HERE, the tracker, open items, and a new log entry.
2. **The log is append-only.** Correct a wrong entry with a dated note under it.
3. **Verify before writing.** Run `git -C ~/chukta log --oneline` and `git status`, and read the files. Committed, written-but-uncommitted and planned are three different states.
4. **Organise by what a thing is** (component, decision, bug), not by who asked. A list organised by requester goes stale once the work ships under a component's name.
5. **Long procedures get their own file** under `docs/` and a row in the document map. This file stays readable.
6. **No secrets, keys, tokens or real personal data** in any doc. v1 is synthetic only.

| If you changed | Update here |
|---|---|
| Anything | A log entry at the top of Part 2, dated, with the commit SHA |
| A decision | The decision register row and the ADR file |
| A bug or doc defect | A bug register row, with symptom and root cause kept separate |
| Scope, phase or plan | START HERE, the tracker, and Plans |
| A statutory claim | PRD Appendix A |

## Document map

**This file is the index of what happened and why.** If you add a document, add a row.

| If you need | Read | Status |
|---|---|---|
| The original requirements | [`BRIEF.md`](BRIEF.md) | Frozen |
| What we build, for whom, and how we measure it | [`00-PRD.md`](00-PRD.md) | Frozen for step 2 review |
| Why a decision was made | [`adr/`](adr/): ADR-0001 to ADR-0019, summarised in the decision register below | Written, In Review |
| System architecture: components, trust zones, flows, durable execution, model gateway, deployment | [`01-ARCHITECTURE.md`](01-ARCHITECTURE.md) | Frozen for step 2 review |
| Data model | `02-DATA-MODEL.md` | Pending (Hat A) |
| Agent and engine design | `03-AGENT-DESIGN.md` | Pending (Hat A) |
| Retrieval design | `04-RAG-DESIGN.md` | Pending (Hat A) |
| Evaluation plan | `05-EVAL-PLAN.md` | Pending (Hat A) |
| Threat model: threats, deep dives, security decisions S-01 to S-14, test suites, VERIFY register V25 onward | [`06-SECURITY-THREAT-MODEL.md`](06-SECURITY-THREAT-MODEL.md) | In Review |
| API contracts, including the send adapter and Tally adapter interfaces | `07-API-CONTRACTS.md` | Pending (Hat B) |
| Synthetic data | `08-SYNTHETIC-DATA-SPEC.md` | Pending (Hat B) |
| The 28-day build plan | `09-BUILD-PLAN.md` | Pending (Hat B) |
| Repo layout for the build | `10-REPO-STRUCTURE.md` | Pending (Hat B) |
| Test strategy, including the doc lint that DOC-01 to DOC-03 call for | `11-TEST-STRATEGY.md` | Pending (Hat B) |
| Data classification: classes, inventory, handling, minimisation, retention, residency, synthetic-data rules, pilot gate | [`12-DATA-CLASSIFICATION.md`](12-DATA-CLASSIFICATION.md) | In Review |
| Reviews between hats | `reviews/` | Pending |
| Full index | `INDEX.md` | Final step |

---

# Part 2: Record

## Dated log (newest first)

### 2026-09-11 · 12-DATA-CLASSIFICATION written (Hat C)
- Six classes (DC-0 to DC-5), plus a trust label kept separate from class. User-typed free text counts as untrusted for prompts.
- An inventory of 23 data items, each with its class, storage, hosted-model eligibility, retention and erasure path. Statutory retention periods are tagged, never stated.
- Minimisation at import for WhatsApp exports, bank statements and email.
- Residency: ap-south-1 for the production target, answering A-Q7 together with `06` S-14.
- Synthetic data is built to be impossible to confuse with real data (invalid check characters, reserved domains, non-mobile ranges), and real-looking data is flagged.
- A 13-point pilot-readiness gate that must pass before ADR-0015's real-data gate lifts.

### 2026-09-11 · 06-SECURITY-THREAT-MODEL written (Hat C)
- 24 threats, each with an inherent and a residual severity, and six adversaries, the model itself among them.
- Deep dives on the brief's eight required areas. The prompt-injection defence rests on capability, not prompt wording: a successful injection cannot change state, pick a tool, switch tenant or reach outbound prose. The citation gate is formalised as stages G1 to G7. It proves provenance, not applicability, and it says so.
- Every approval-gate bypass path is listed (P-1 to P-9). Two new ones were found: Razorpay sending links or reminders itself, and an MCP client sending an unapproved draft. Both are closed. A person copying a pending draft (P-9) is the one accepted residual.
- Security decisions S-01 to S-14 answer `01` A-Q3, A-Q4, A-Q7 and A-Q8. MCP gets option (a), tightened (S-01).
- 15 test suites (ST-01 to ST-15) are handed to Hat B for `11`.
- VERIFY register V25 to V43, covering DPDP, the SPDI Rules, CERT-In, and GST and income-tax record retention.

### 2026-09-11 · 01-ARCHITECTURE approved, owner findings F-01 to F-05 applied
- **F-01 (High):** `01` now states the MCP versus P9 contradiction outright, and A-Q8 lists three resolutions. Hat A recommends (a): MCP returns only pseudonymised structured records, with no raw message or document text. Hat C decides in `06`.
- **F-02 (Medium):** the MCP server was folded into the Console as a route. That removes a container, the MCP-to-Console token type and a second public surface. D-02 was reworded.
- **F-03 (High):** v1 runs on `docker compose up` on a developer machine. A GPU instance in ap-south-1 costs roughly $900 to $1,100 a month on demand (owner's estimate), which this project will not pay. The AWS topology is kept as a specified-but-not-built target. D-03 holds under Compose networks, and the egress property is now tested on every CI push. D-06 was revised to native builds. One refinement to the owner's note: besides the proxy, nginx also sits on a routable network, because Docker cannot publish a port from an internal-only one. It runs no application code and cannot reach the data network.
- **F-04 (Low):** approval-queue depth and age signal, with an alert. The PRD has no matching requirement yet (O-10).
- **F-05 (Low), check only:**
  - Commit `078b7d4` touched only the PRD. Its §10 rows went from `01..05, 23, 06..22` to `01..24`.
  - All 56 PRD references moved under the single old-to-new mapping, in order, with only the two new SM-24 references added.
  - All 61 SM references in files the script never touched (`01`, the ADRs, this file, the brief) resolve to the metric they mean.
  - The one old-number citation is DOC-01's symptom. It is history, and it now carries a dated note.
- **Sequence change:** the owner moved Hat C ahead of Hat A's `02` to `05` (O-11).
- `01` status: Frozen for step 2 review. All 21 Mermaid diagrams in the PRD and `01` parse.

### 2026-09-11 · 01-ARCHITECTURE written (Hat A)
- Written at the owner's go and committed with this update. It covers nine principles, the system context, six trust zones with egress only from the compute zone, component responsibilities with a "must never" column, four runtime sequences, durable execution, data stores, model routing and pseudonymisation, security hooks, deployment, observability and cost, NFR traceability, failure modes and extension points.
- Nine decisions were made in it (D-01 to D-09), recorded with reasoning in `01` §15 and tracked as O-08 until they become ADRs. Its seven open questions (A-Q1 to A-Q7) are tracked as O-09.
- Checked before commit: no dashes, and every ADR, requirement and D- ID cited in `01` resolves. All 20 Mermaid diagrams in the PRD and `01` parse with Mermaid 11.4.1. The 11 PRD diagrams had never been machine-checked before.
- 🛑 STOP. 02-DATA-MODEL waits for the owner's go.

### 2026-09-11 · PRD fixes applied, master doc brought current, STOP
- DOC-01, DOC-03, GAP-01 and GAP-02 applied to the PRD in `078b7d4`. The fixes ran as one script that aborts unless every anchor matches exactly once. Six assertions passed before the commit: SM rows in reading order 01 to 24; every SM reference in the PRD and the ADRs resolves; the inline VERIFY set equals Appendix A; no dashes; the FR-HQ, NFR-14 and SM-24 rows exist; the new references resolve.
- PRD status set to Frozen for step 2 review.
- This file rewritten to match: O-01 and O-02 closed, bug register filled, decision register linked to the ADR files.
- 🛑 STOP. 01-ARCHITECTURE waits for the owner's go.

### 2026-09-11 · ADRs written, repo on GitHub, Claude attribution off everywhere
- ADR-0001 to ADR-0019 written and committed in `75cc43c`. This closes DOC-02.
- **Commit identity settled (O-01).** The owner pointed at the `monarchsolanki` GitHub account, and commits use that account's noreply address, set repo-locally. The owner's first answer, "same as the LMS private repo", would have meant the college address, because every LMS commit uses it. That was flagged and not used.
- **Private repo `monarchsolanki/chukta` created and pushed (O-02).** First commits: `947cc5a` brief, `e50d320` approved PRD.
- **The owner ruled that no commit or PR in any project may credit Claude.** Claude Code's global `attribution` setting now holds empty strings, and ADR-0019 records the rule. Existing Claude trailers in other repos were found (1 in setu-tss-docs, 17 in the website repo) and left alone. Rewriting shared history needs the owner's explicit go.
- The session paused twice for the owner's connectivity. START HERE recorded the resume point both times.

### 2026-09-11 · PRD approved at the checkpoint
- The owner approved the PRD and raised two defects and two gaps: DOC-01 (metric numbering), DOC-02 (ADR files missing), GAP-01 (no model-spend circuit breaker) and GAP-02 (the human review queue was undefined). Hat A found DOC-03 (two VERIFY IDs never used inline) in its own lint.
- The owner set this step's scope: ADRs and PRD fixes, then STOP.

### 2026-09-11 · Checkpoint 1 decisions, repo moved out of N073, PRD started
- All 19 Hat A recommendations were adopted as ADR-0001 to ADR-0019. Six were modified (see the decision register).
- The repo was moved from `~/N073/chukta` to `~/chukta` at the owner's request, because nesting it inside the company docs repo was an accident risk. N073's `.git/info/exclude` was restored and verified byte-identical to git's stock template. N073's `git status` is clean and its HEAD is unchanged at `ed339c0`.
- The owner will give a personal commit address before the first commit (O-01).
- `BRIEF.md` was saved verbatim with status Frozen.
- `00-PRD.md` §1 to §4 are written. The rest is in progress.
- This master document was created, following the LMS master-doc pattern and its lessons.

### 2026-09-11 · Project started
- The brief was received. Hat A posted a five-bullet understanding, 19 disagreements and a PRD table of contents, then stopped.
- The repo was created at `~/N073/chukta` with `git init -b main` and excluded from the SETU docs repo with a local exclude line. *Superseded the same day by the move above.*

## Decision register

One row per ADR. The ADR file holds the full reasoning, and this row holds the one-line why. All rows were accepted at checkpoint 1 on 2026-09-11. The files are in [`adr/`](adr/), committed in `75cc43c`.

| ADR | Decision | Why, in one line | Modified at checkpoint 1 |
|---|---|---|---|
| [0001](adr/ADR-0001-43bh-year-end-deferral.md) | The 43B(h) rule is a year-end deferral, modelled as named behaviour B-1 | A June notice claiming a lost deduction would be false | Yes. B-1 is a named PRD behaviour, and the ladder changes shape from January to March. |
| [0002](adr/ADR-0002-clock-starts-at-acceptance.md) | The statutory clock starts at acceptance, and disputes can reset it | The statute counts from acceptance, not from the invoice date | No |
| [0003](adr/ADR-0003-eligibility-and-subset-positioning.md) | Eligibility is decided per invoice, with reason codes | Medium enterprises, traders and some buyers fall outside the levers | Yes. Evidence and Reconciliation are the primary product. Statutory is a subset feature. |
| [0004](adr/ADR-0004-s16-method-parameters.md) | s.16 method parameters are versioned, and the bank rate sits in an effective-dated table | The statute leaves method choices open | No |
| [0005](adr/ADR-0005-owner-verified-statute-text.md) | The owner supplies verified statute text, and unverified provisions cannot be cited | No statute text from memory | Yes. Loaded in Phase 4, with a checklist in PRD Appendix B. |
| [0006](adr/ADR-0006-s18-cited-no-filing.md) | Notices cite s.18, with no filing workflow | Gives the notice a next step without building a legal workflow | No |
| [0007](adr/ADR-0007-deterministic-orchestrator-and-engines.md) | The Orchestrator, Statutory and Analyst roles are deterministic code | Code does these without loss, and a model adds error and nondeterminism | No |
| [0008](adr/ADR-0008-statutory-corpus-not-rag.md) | The statutory corpus is a verified table, not RAG | Retrieval would add a wrong-section failure to the worst path | No |
| [0009](adr/ADR-0009-case-per-customer-account.md) | One case per customer account, with a state machine per invoice inside it | Replies and payments arrive per customer. Avoids parallel drafts to one contact. | No |
| [0010](adr/ADR-0010-citation-gate-typed-slots.md) | The citation gate uses typed slots and a deterministic scanner | "Claim" cannot be defined by a model | No |
| [0011](adr/ADR-0011-no-send-in-v1.md) | No send capability in v1 | Bypassing the gate becomes impossible by construction | Yes. The send adapter is specified in 07 with its harder gate. Razorpay test-mode links and signed webhooks are in scope. |
| [0012](adr/ADR-0012-tenant-isolation-two-layers.md) | Tenant isolation through the retrieval API plus Postgres RLS. Tenant is bound, never a tool argument. | Two enforcement points, and the model cannot choose the tenant | No |
| [0013](adr/ADR-0013-celery-wake-at-outbox.md) | Celery for compute, `wake_at` rows in Postgres for timers, and an outbox | Long Celery ETAs on Redis get redelivered, and Postgres and Redis cannot commit atomically | Yes. The Postgres-only queue is rejected because it would need a hand-rolled worker pool for compute. |
| [0014](adr/ADR-0014-separate-agent-runtime-schema.md) | Agent-runtime tables live in a separate Postgres schema | Prisma treats unknown tables as drift and offers a reset | No |
| [0015](adr/ADR-0015-langfuse-cloud-with-masking.md) | Langfuse Cloud for v1, with trace masking and a real-data gate | Self-hosted v3 needs ClickHouse, Redis and S3 | No |
| [0016](adr/ADR-0016-model-routing-privacy-first.md) | The local SLM is justified by privacy. The frontier model sits behind an interface. Cost includes GPU time. | Local inference is not automatically cheaper at pilot volume | No |
| [0017](adr/ADR-0017-eval-data-sources.md) | A human-written Hinglish set, generation by a different model family, and metrics split by source | Generation by the same model family inflates scores | Yes. The owner and 2 or 3 others write 80 messages. Never report a blended number. |
| [0018](adr/ADR-0018-review-sequence.md) | Hat C writes 06 and 12 at step 2, with a delta pass after step 4 | Otherwise the threat model describes the design before revision | No |
| [0019](adr/ADR-0019-repo-conventions.md) | Repo conventions: location, noreply identity, no Claude attribution, one commit per deliverable, dual ADR status, verbatim brief | Traceability, and the owner's standing rules | Extended 2026-09-11 with the identity and attribution rules |

## Bug register

A wrong fact or a missing requirement in a doc is a defect. IDs use `DOC-` for doc defects and `GAP-` for missing requirements, as the owner named them.

| ID | Found | Severity | Symptom | Root cause | Fix (commit) | Regression test | Status |
|---|---|---|---|---|---|---|---|
| DOC-01 | 2026-09-11, owner review | Low | In PRD §10, SM-23 sat in the Safety invariants block but was numbered after the Efficiency block, so IDs did not run in reading order | The exactly-once metric was added after the table was numbered, and it took the next free number instead of being inserted and renumbered | `078b7d4`: SM-06 to SM-23 renumbered by script, and every reference in the PRD updated. The ADRs already used the new numbers. | Doc lint (planned in `11`): SM IDs ascend through the §10 tables, and every SM reference resolves to a row | Fixed |
| DOC-02 | 2026-09-11, owner review | High | The decision register and PRD Appendix C cited ADR-0001 to 0019, but no ADR files existed. If the session had ended, the reasoning would have been lost. | The ADRs were scheduled at the end of Hat A's set, while the PRD and this file were written to cite them first. Nothing checked that a cited file existed. | `75cc43c`: all 19 ADRs written | Doc lint: every `ADR-00nn` reference has a file | Fixed |
| DOC-03 | 2026-09-11, Hat A lint | Low | PRD Appendix A listed V02 as used in §5.8 and V13 in FR-STA-3, but no inline tag used either | Appendix A was written from the intended claim list, not generated from the tags actually in the text | `078b7d4`: V02 tagged in §5.8, V13 tagged in FR-STA-3 | Doc lint: the inline VERIFY set equals the Appendix A set | Fixed |
| GAP-01 | 2026-09-11, owner review | High | Nothing stopped a runaway agent loop on the frontier model. NFR-12 covered API rate limits, not model spend. | Cost was treated as a number to report (SM-22), not a limit to enforce | `078b7d4`: NFR-14, a per-case spend circuit breaker, measured by SM-24 | SM-24 runaway-loop suite | Fixed in spec |
| GAP-02 | 2026-09-11, owner review | High | FR-ING-4 and FR-CNV-3 routed to "a human queue" that no requirement defined: what enters, who works it, what it blocks, what happens when an item goes stale | Low-confidence routing was written as an exit on each flow. The queue itself was never specified as a component. | `078b7d4`: FR-HQ-1 to FR-HQ-6 | FR-HQ acceptance tests, detailed in `11` | Fixed in spec |
| F-01 | 2026-09-11, owner review of `01` | High | P9 says raw counterparty content stays on our infrastructure, but the MCP endpoint hands data to the user's own MCP client, usually an LLM app pointed at a hosted model. That path bypasses the gateway, the pseudonymiser and trace masking. | P9 was written around the model gateway. The MCP component came from the brief's stack list and was never checked against the principles. | Contradiction stated in `01` §2. A-Q8 added with three resolutions and a recommendation. Hat C decides in `06`. | Principle-by-component check in the doc lint (planned in `11`) | Open until `06` decides |
| F-02 | 2026-09-11, owner review of `01` | Medium | D-02 called the Console the only public application surface, while §3 routed Nginx to a separate MCP server too | D-02 was written about webhooks and the §3 diagram from the component list. Nothing cross-checked decision text against the diagrams. | MCP server folded into the Console as a route. D-02 reworded. §3, §4, §9 and §10 updated. | Doc lint: every public route in the diagrams belongs to a component that D-02 names | Fixed |
| F-03 | 2026-09-11, owner review of `01` | High | §10 assumed a funded AWS deployment with a GPU host, and A-Q5 treated the GPU host as a scheduling question when it is not affordable at all | Cost was never checked against the project's budget before the topology was drawn | v1 is `docker compose up` on a developer machine, and AWS is a specified-but-not-built target. D-03 shown to hold under Compose networks and tested in CI. A-Q5 reframed. D-06 revised. | CI egress and network tests (`01` §10.1) | Fixed |
| F-04 | 2026-09-11, owner review of `01` | Low | §11 alerted on review-queue age but not on approval-queue age, so a draft nobody approves would stall its case unnoticed | Ageing was specified for the review queue (FR-HQ-5) and not generalised to every queue | Approval-queue signal and alert in `01` §11. The PRD requirement is O-10. | Alert test in `11` | Fixed in `01`. PRD requirement open (O-10). |

Symptom and root cause are separate columns on purpose. In one LMS batch, every root cause turned out to differ from the reported symptom.

*2026-09-11 note (F-05):* DOC-01's symptom cites "SM-23" in the old numbering on purpose, because it describes the defect. That metric is now SM-06. Every other SM reference in every document resolves to the metric it means (see the log).

## Plans

**Documentation phase (now):** the checkpoint tracker in Part 1. Hat C (step 2) is next, ahead of Hat A's 02 to 05 at the owner's request. Those follow, with a Hat C delta pass (O-11).

**Build phase:** 28 days, one developer. The phases are defined by Hat B in `09-BUILD-PLAN.md`. One fixed point is already known: statute text is loaded in Phase 4.

**After v1.** Out of scope, recorded here so they are not lost:

| Item | Why deferred | Needs first |
|---|---|---|
| Implement the send adapter | NG1 | The harder gate specified in 07 |
| Implement the Tally adapter | NG3 | The contract in 07 |
| A real pilot | Business outcome metrics need one | DPDP obligations (12), live payment keys, a new ADR replacing ADR-0015's real-data gate |
| Live Razorpay keys | NG7 | The pilot |
| Account Aggregator or bank feeds | NG8 | Consent flows |
| MSEFC or Samadhaan filing workflow | NG6 | Legal review |
| More languages | Out of v1 language scope | A human-written eval set per language |

## Lessons carried over from the LMS project

- The master doc is trusted only if every ship updates it. The LMS one once went three weeks stale on versions while still reading as authoritative.
- Symptom and root cause differ more often than not. Record both.
- Organise by what a thing is, not by who asked.
- Check working trees, not just `main`. Uncommitted work is invisible in a log.
- Prisma's migrate drift detection will offer to reset a database it doesn't fully own (ADR-0014).

## Lessons from this project

- **A reference is not a file.** The PRD cited 19 ADRs that did not exist yet (DOC-02). Lint references, not just prose.
- **Generate registers from the text.** Appendix A drifted from the inline tags as soon as it was written by hand (DOC-03).
- **Check the premise behind an answer before acting on it.** "Use the same email as the LMS repo" pointed at the one address the owner had ruled out.
- **Script mechanical edits, and make them all-or-nothing.** The SM renumber touched about 50 references. It ran as one script that refused to write unless every anchor matched exactly once.
- **Check every component against every principle.** P9 was written around the model gateway, and the MCP endpoint was never tested against it (F-01).
- **Cross-check decision text against the diagrams.** D-02 and the §3 diagram disagreed for a whole review cycle (F-02).
- **Price the topology before drawing it.** The GPU host was designed in before anyone asked whether it was affordable (F-03).
- **Apply a control to every instance of its pattern.** Ageing was specified for one queue and missed for the other (F-04).
