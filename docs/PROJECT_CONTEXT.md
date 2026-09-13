# Chukta: Project Context (master document)

| | |
|---|---|
| **Purpose** | The single master record for Chukta: current state, what is open, every decision and why, every bug and its cause, and what comes next. |
| **Intended reader** | Anyone picking the project up, including a future session with no memory of this one. Read Part 1 first. |
| **Status** | Draft. This file is never frozen. It is updated in the same commit as every deliverable. |
| **Last updated** | 2026-09-13 |

**Two parts, and they age in opposite directions.**

| Part | Contents | Rule |
|---|---|---|
| **Part 1: Now** | START HERE, what is open, traps, how to keep this file current, document map | **Rewrite** whenever anything changes |
| **Part 2: Record** | Dated log, decision register, bug register, plans, lessons | **Append only.** Fix a wrong entry by adding a dated correction under it. Never edit history silently. |

---

# Part 1: Now

## START HERE (one screen, verified 2026-09-13)

- **Phase:** design documentation only. No application code until the doc set is complete ([BRIEF](BRIEF.md), "Your task in this session").
- **Current step:** `02` and `03` are approved. O-06's fit test ran on 2026-09-13, and **`qwen3.6:27b` failed its pre-set rule** on the developer's machine, even with Docker idle (O-06 row, and the log). `05` stays blocked until the owner picks the local model.
- **v1 in one line:** the four claims (tenant isolation, citation gate, no send path, spend cap) tested by build day 12, the statutory engine as the gate's carrier, and two AI slices, Reconciliation and Conversation, in 24 working days (ADR-0031). **Phase 2** follows: 8 days with a hard stop, the dispute agent with its RAG layer (ADR-0032).
- **🛑 STOP.** Waiting for the owner's O-06 decision after the failed fit test. Hat A's recommendation is the pre-agreed fallback, `qwen3:14b`: a 9.3 GB download, re-tested under the same rule. Then `05`, Hat C's delta 2 over `02`, `03` and `05`, and Hat B's `07` to `11`.
- **Needs the owner:** O-06, the local model, after the fit test.

| | Verified 2026-09-13 |
|---|---|
| Repo | `~/chukta`, a sibling of `~/N073` and **not inside it**. Branch `main`, pushed to **public** GitHub repo <https://github.com/monarchsolanki/chukta> (made public 2026-09-13 at the owner's request). Commits use the owner's GitHub noreply address, set repo-locally, and never credit Claude. |
| Commits | `947cc5a` brief · `e50d320` PRD · `75cc43c` ADR-0001 to 0019 · `078b7d4` PRD fixes · `a9d8893` this file · `e212bdd` `01` · `762d8b7` F-01 to F-05 · `ac21264` `06` · `6a7fd5d` `12` · `7c66d16` SEC-REVIEW-ARCH · `1dad35d` SEC-01 · `2366294` feasibility review · `a213c8b` ADR-0020 to 0035 · `f9ca1c2` step-4 revisions · `7a60972` delta 1 · `cef46cc` repo made public · `76d0eb7` O-17 and O-19 decisions · `ef1fc8a` C-1 and C-2 ADRs · `bb02208` C-1 and C-2 revisions · `b7d5dbb` `02` · `3746042` `03` · then OD-4, FB-03 and DOC-04 |
| Written | `BRIEF.md` (Frozen). This file. `00-PRD.md`, `01-ARCHITECTURE.md`, `06-SECURITY-THREAT-MODEL.md` and `12-DATA-CLASSIFICATION.md` (revised 2026-09-13, frozen for delta 2). ADR-0001 to 0040 (In Review, all Accepted). `02-DATA-MODEL.md` and `03-AGENT-DESIGN.md` (approved 2026-09-13, revised by ADR-0040, frozen for delta 2). `reviews/SEC-REVIEW-ARCH.md` and `reviews/IMPL-FEASIBILITY-REVIEW.md` (approved). |
| Pending | O-06 (the owner). Then Hat A's 05, Hat C's delta 2 over 02, 03 and 05, and Hat B's 07 to 11, including Phase 2's paragraph in 09 (O-18) and the demo driver in 09 and 10 (O-26). Then README and INDEX. `04` waits for Phase 2. |
| Statutory corpus | Empty by design. The owner loads verified text by build day 21 (ADR-0033, PRD Appendix B). |
| Running system | None. v1 is not built. |

**Checkpoint tracker** (brief §3, as changed by ADR-0035)

| Step | Hat | Deliverables | Status |
|---|---|---|---|
| 0 | A | Understanding, 19 recommendations, PRD TOC | ✅ Done 2026-09-11 |
| 1 | A | 00, 01, ADR-0001 to 0019 | ✅ Approved 2026-09-11. The owner moved `02` to `05` after step 4. |
| 2 | C | 06, 12, SEC-REVIEW-ARCH | ✅ Approved 2026-09-11, plus the owner's SEC-01 |
| 3 | B | IMPL-FEASIBILITY-REVIEW | ✅ Approved 2026-09-11, with OD-1 to OD-3 decided |
| 4 | A | Revision: ADR-0020 to 0035 and document revisions | ✅ Done 2026-09-12 |
| 4b | C | Delta 1 over the step-4 revision | ✅ Done 2026-09-12. 6 new findings (DLT-01 to DLT-06), one High. Awaiting the owner's review. |
| 5a | A | 02 and 03, then 05. `04` moved to Phase 2's first day. | ✅ `02` and `03` approved 2026-09-13. `05` is blocked on O-06. |
| 5b | C | Delta 2 over 02, 03 and 05 | Not started |
| 5c | B | 07 to 11 | Not started |
| 6 | Final | README, INDEX | Not started |

## What is actually open (verified 2026-09-13)

Closed at step 4 on 2026-09-12: O-08 (ADR-0020 to 0023), O-09 (`01` §16 status table), O-10 (FR-APR-3), O-13 (ADR-0020 to 0030), O-15 (the owner's decisions) and O-16 (the step-4 revisions). O-01 and O-02 closed on 2026-09-11. O-17 and O-19 closed on 2026-09-13 by the owner's decisions. O-21 closed on 2026-09-13 by ADR-0036 to ADR-0038 and the feasibility review §11. O-22 closed on 2026-09-13 by the revision of `01`, the PRD, `06` and `12`. O-14 closed on 2026-09-13 by `02` §3. O-24 closed on 2026-09-13 by ADR-0039.

| ID | Item | Owner | Due or blocks |
|---|---|---|---|
| O-03 | Verify V01 to V24 (PRD Appendix A) by elapsed build day 21, alongside O-04 (ADR-0033). Verify V25 to V43 (`06` Appendix A) before the pilot gate. | Monarch | Build day 21, and the pilot gate |
| O-04 | Load verified statute text per PRD Appendix B | Monarch | Build day 21 |
| O-05 | Hand-write 80 Hinglish messages, with 2 or 3 other contributors | Monarch | Build day 18 |
| O-06 | **Blocks `05`.** Choose the local model. **Fit test, 2026-09-13, on `qwen3.6:27b`, with the pass rule set in advance (100% of the model on the GPU, and no swap growth during a 15-call batch at 8,192 tokens of context): FAIL in both runs.** Run A, with Docker's VM idle: 86.8% on the GPU, swap from 0 to 6.1 GB on load and +4.7 GB during the batch, 17.9 tokens per second, 15 of 15 valid JSON. Run B, with Postgres plus a 6.5 GiB fill inside Docker's 7.75 GiB VM: 82.3% on the GPU at worst, +3.7 GB of swap during the batch, 14.6 tokens per second, 12 of 15 valid JSON. Machine: Apple M5 Pro, 25.8 GB unified memory, Metal GPU budget 19.07 GB. The pre-agreed fallback, `qwen3:14b` (Q4_K_M, 14.8B, 9.3 GB), is not downloaded yet. | Monarch | `05` |
| O-07 | Default CA-review policy for formal notices. Proposed: required by default, and the owner can waive it with an audit event. | Monarch | PRD §7.2 |
| O-11 | Hat C's delta 2 over `02`, `03` and `05` (ADR-0035) | Hat C | After `05` |
| O-12 | Delta 1 verified every resolution. Five are decided in ADRs but not yet visible in a document: SR-04, SR-06, SR-15, SR-18 and SEC-01. They are checked in `02` to `04` (delta 2) and `10`. SR-10 stays open until build-if-time item 4, or DF-20 at the pilot gate. | Hat C | Delta 2 |
| O-18 | Phase 2's paragraph in `09-BUILD-PLAN.md`: a named follow-on of 8 days with a hard stop, not an open backlog (ADR-0032) | Hat B | Step 5c |
| O-20 | Doc-lint rules in `11`. **Control symmetry (DLT-05):** when a control is decided for one direction or one instance of a pattern, every other instance must be named as covered or explicitly excepted. **Section references (DOC-04):** every reference matches an exact heading, including list continuations, and pointers to "decisions" or "open questions" land on headings with those titles. | Hat B | `11` |
| O-23 | Confirm the Gemini API's current free-tier limits and data terms, and the paid tier's non-training terms, before any figure or claim about them appears in a document. Limits live in configuration (ADR-0036, ADR-0037). | Monarch | Build day 4, with O-06 |
| O-25 | Promote `02`'s D2-01 to D2-07 and `03`'s D3-01 to D3-07 to ADRs at the next Hat A revision, and answer the open questions: D2-Q1 (LangGraph tables, Hat B in `10`), D2-Q2 and D3-Q2 (checkpointer and interrupts inside the wrapper's transaction, the build-day-9 spike), D2-Q3 (column encryption, Hat C in delta 2), D3-Q1, D3-Q3 and D3-Q4 (confidence thresholds, language detection, and a local-prose arm in the blind drafting comparison, Hat A in `05`) | Hat A, Hat B, Hat C | Next Hat A revision |
| O-26 | Record the demo driver (ADR-0039) in `09` (its place in the build, replacing B13's upload pages) and `10` (its location in the repository, and the README's two-command demo) | Hat B | `09`, `10` |

## Traps: easy to get wrong

- **The repo is public.** Anything committed is published. No secrets, real personal data, or anything about other projects beyond what is already here.
- **Never recreate the repo inside `~/N073`.** N073 pushes to the SETU company org. Chukta was nested there for a few minutes on 11 Sep and was moved out.
- **Never `git add -A` in `~/chukta`.** macOS drops `.DS_Store` files, and the repo has no `.gitignore`, because the brief allows only `docs/` and `README.md`. Add paths explicitly.
- **No Claude attribution on any commit or PR, in any project.** This is the owner's standing rule. Claude Code's global `attribution` setting in `~/.claude/settings.json` is set to empty strings. Check new commit messages before every push.
- **`set -e` does not stop a gate script here.** A failing python check did not halt a lint-then-commit script on 2026-09-11. Every check gets its own `|| exit 1`.
- **zsh does not word-split `$VAR`.** A check written as `grep ... $FILES`, with several paths in one variable, greps one nonexistent file and reports a false pass. List the paths or loop over them. This hid the dash check on `f9ca1c2` until delta 1 re-ran it.
- **`BRIEF.md` is verbatim and frozen.** Corrections go in ADRs and PRD Appendix C, never into the brief.
- **"Three-way match" is the buyer's PO/GRN/invoice check.** Our matching is "ledger reconciliation".
- **The 43B(h) rule is a year-end deferral, not a loss.** Never write "lost" (PRD §5.8).
- **No statute from memory.** Every section, rate or deadline is tagged `[VERIFY Vnn: ...]` and listed in PRD Appendix A. The inline set and the Appendix A set must match exactly (DOC-03).
- **SM IDs run in reading order.** A new metric goes at the end of its block, or the whole table is renumbered by script and every reference updated (DOC-01). Old numbers still circulate in instructions written before DOC-01 (O-17).
- **No em dashes in the docs.**
- **v1 is free-tier only** (ADR-0036). A free hosted tier may use what it receives, which is acceptable only because hosted prompts carry nothing but typed facts and templates (ADR-0037). Real data needs a paid tier with non-training terms.
- **The Console never reads the database directly** (ADR-0038). It calls `/api/v1`, like any future mobile client.
- **Prisma must not manage LangGraph's tables** (ADR-0014), **and no running service may connect as the migration role** (ADR-0027).
- **v1 has no GPU host and no AWS deployment** (F-03). It runs on `docker compose up` on a developer machine. The AWS topology in `01` §10.3 is a specified target, not something to build.
- **Payment details are regulated tokens** (SR-01, ADR-0024). An account number, UPI ID, IFSC code, URL, email or phone in outbound prose must come from a slot, never from model text.
- **In v1, hosted model calls are drafting prompts only** (ADR-0021). A free-text hosted call waits for DF-14's full pseudonymisation pipeline.
- **Phase 2 is 8 days with a hard stop, not a backlog** (ADR-0032). Unfinished work goes back to `12` §9.2.
- **`05` waits for O-06.** Three of the four model nodes run only on the local SLM (`03` §7), so the model choice moves the targets `05` sets. `qwen3.6:27b` failed its fit test on the developer's machine.
- **Never record a decision as the owner's without the owner's words in the record** (O-17).

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
| What we build, for whom, and how we measure it | [`00-PRD.md`](00-PRD.md) | Revised 2026-09-13. Frozen for delta 2. |
| Why a decision was made | [`adr/`](adr/): ADR-0001 to ADR-0040, summarised in the decision register below | Written, In Review, all Accepted |
| System architecture: components, trust zones, flows, durable execution, model gateway, deployment | [`01-ARCHITECTURE.md`](01-ARCHITECTURE.md) | Revised 2026-09-13. Frozen for delta 2. |
| Data model: tables, the two database roles and their grants, tenant isolation down to checkpoints, database-enforced invariants | [`02-DATA-MODEL.md`](02-DATA-MODEL.md) | Approved 2026-09-13. Revised by ADR-0040. Frozen for delta 2. |
| Agent and engine design: the case graph, router, nodes, input contracts, engines, durable execution | [`03-AGENT-DESIGN.md`](03-AGENT-DESIGN.md) | Approved 2026-09-13. Revised by ADR-0040 and DOC-04. Frozen for delta 2. |
| Retrieval design | `04-RAG-DESIGN.md` | Written on Phase 2's first day (ADR-0032, ADR-0035) |
| Evaluation plan, including SM-25's blind drafting comparison (ADR-0040) | `05-EVAL-PLAN.md` | Blocked on O-06 (Hat A, step 5a) |
| Threat model: threats, deep dives, security decisions and their v1 status, test suites, VERIFY register V25 onward | [`06-SECURITY-THREAT-MODEL.md`](06-SECURITY-THREAT-MODEL.md) | Revised 2026-09-13. Frozen for delta 2. |
| API contracts: the public JSON API and token model (ADR-0038), plus the send adapter and Tally adapter interfaces | `07-API-CONTRACTS.md` | Pending (Hat B) |
| Synthetic data | `08-SYNTHETIC-DATA-SPEC.md` | Pending (Hat B) |
| The build plan: v1 in 24 days with the demo driver (ADR-0039), then Phase 2 | `09-BUILD-PLAN.md` | Pending (Hat B) |
| Repo layout for the build | `10-REPO-STRUCTURE.md` | Pending (Hat B) |
| Test strategy, including the doc lint that DOC-01 to DOC-03 call for | `11-TEST-STRATEGY.md` | Pending (Hat B) |
| Data classification: classes, inventory, handling, minimisation, retention, residency, synthetic-data rules, pilot gate | [`12-DATA-CLASSIFICATION.md`](12-DATA-CLASSIFICATION.md) | Revised 2026-09-13. `12` §9.1 has 14 conditions, and `12` §9.2 lists DF-01 to DF-16. |
| Reviews between hats | [`reviews/SEC-REVIEW-ARCH.md`](reviews/SEC-REVIEW-ARCH.md): 18 findings plus SEC-01, and the delta passes. [`reviews/IMPL-FEASIBILITY-REVIEW.md`](reviews/IMPL-FEASIBILITY-REVIEW.md): what fits in 28 days, the cuts, the build order. | Both approved. Delta 1 done 2026-09-12. |
| Full index | `INDEX.md` | Final step |

---

# Part 2: Record

## Dated log (newest first)

### 2026-09-13 · O-06 fit test: `qwen3.6:27b` fails on the developer's machine
- The owner approved the 17.8 GB download and the test. **The pass rule was set before the test ran:** with the stack running, every sample shows the model 100% on the GPU, and swap does not grow during a 15-call batch (10 Hinglish and English classifications, 5 ledger-layout inferences, 8,192 tokens of context). No tolerance was added afterwards.
- v1's stack does not exist yet. Run B used a stand-in built only from images already on the machine: a real Postgres container, plus a Node container holding 6.5 GiB inside Docker's 7.75 GiB VM, which is the most memory the stack could take. Run A, with Docker's VM idle, was the baseline.
- **Both runs failed.**
  - **Run A:** 86.8% of the model on the GPU (15.61 of 17.97 GB). Loading pushed swap from 0 to 6.1 GB, and the batch added 4.7 GB. About 18 tokens per second, and 15 of 15 valid JSON.
  - **Run B:** 82.3% on the GPU at worst, with swap growth of 3.7 GB during the batch. About 15 tokens per second, and only 12 of 15 valid JSON. During the batch Ollama's allocation for the model grew from 17.97 to 21.66 GB, with 17.82 GB on the GPU. The cause is not yet explained.
- **The model works when it runs, so the failure is memory, not capability.** On this 25.8 GB machine the model never sat fully on the GPU, even with nothing but an idle Docker VM beside it.
- **Correction, logged here:** mid-test, Hat A reported that run B was still running and would be stopped. It had already finished. Nothing was cut short, and both runs are complete.
- Afterwards the test containers were removed and the model unloaded. The model stays on disk until the owner decides.
- 🛑 STOP. The owner decides O-06.

### 2026-09-13 · `02` and `03` approved; OD-4, FB-03, O-06 moved ahead of `05`; DOC-04
- **The owner approved `02` and `03`,** confirming DLT-02, DLT-03 and SEC-01 as correctly resolved, and named `02` §7 and §4.4 as the strongest work in the set.
- **OD-4, accepted with a change of shape (ADR-0039):** not an upload command, but a demo driver that seeds, ingests a ledger, runs a case to a draft, shows it and approves it, all through `/api/v1`. Contingency returns to 1.0 day. `09` and `10` record it (O-26).
- **FB-03, the owner's finding (ADR-0040):** N-11 is the only hosted path, falls back silently, and nothing measured it. That fails the load-bearing-AI test the project applied to every other component. New SM-25 (drafting fallback rate, per mode), and a blind drafting comparison in `05` with its decision rule written before it runs. If readers prefer nothing, v1 runs local-only. PRD, `02` and `03` revised.
- **O-06 moved ahead of `05`,** because three of the four model nodes run only on the local SLM, and a smaller model moves SM-09, SM-11, SM-13 and SM-14. Facts were measured on the developer's machine, with nothing downloaded (O-06 row).
- **DOC-04:** `03`'s conventions pointed at wrong but existing sections. The lint had accepted any reference whose major section existed. Fixed, and a strict scan run across every document.

### 2026-09-13 · 03-AGENT-DESIGN written (Hat A). STOP.
- **The case graph:** a deterministic router (N-02) that clears incoming information before planning outbound work, 18 v1 nodes, and a typed case state that no model sees.
- **Four model nodes in v1** (N-03 classify, N-05 infer layout, N-07 explain residuals, N-11 draft prose) and fourteen code nodes. N-03, N-05 and N-07 run on the local SLM in both modes (D3-02). Only N-11 can reach a hosted model, and only in hosted mode.
- **Input contracts per model node (§5).** This closes the content-boundary part of SR-06 and SR-15. **DLT-02 is structural:** the drafting builder has no free-text parameter (D3-05), and seller text is rendered into its slot after generation (§5.3).
- **Output validation by code.** Relative dates are resolved by code in IST (D3-04). A deduction is derived from arithmetic, never from a hard-coded rate.
- **Engines:** the matcher, the statutory engine (no statutory number in code, D3-06) and B-1 planning.
- **Durable execution:** idempotent node writes, approval as an interrupt, halts. Quota use is counted when a request is sent (D3-07).
- **Review items by node.** Decisions D3-01 to D3-07 and open questions D3-Q1 to D3-Q3 join O-25.
- 🛑 STOP, as the owner directed.

### 2026-09-13 · 02-DATA-MODEL written (Hat A)
- **Two database roles (SEC-01):**
  - `chukta_migrator` owns everything, and only the one-shot `migrate` and `load-reference` jobs use it.
  - `chukta_runtime` owns nothing and has no `BYPASSRLS`, and every running service uses it.
  - Grants are set table class by table class. Decision records are insert-only. Statute text, rates, method profiles and templates are read-only for runtime.
- **Tenant isolation in the schema:**
  - forced RLS, plus foreign keys that carry `tenant_id`, so no row can point at another tenant's row
  - user-scoped RLS for sign-in
  - checkpoint RLS on the `tenant:case` prefix, plus DLT-03's wrapper, which raises before querying and treats an empty checkpoint for a case that has run as an error
  - five `SECURITY DEFINER` functions that return only IDs, for work that must happen before a tenant is bound
- **DLT-02 in the schema:** approved seller text is a `seller_text` record, rendered as a slot, and no prompt builder has a column to read it from.
- **14 invariants the database enforces** even if code is wrong, among them one case per account, synthetic-only tenants, approvals bound to exact bytes, and share links only after approval.
- Decisions D2-01 to D2-07 and open questions D2-Q1 to D2-Q3 are tracked as O-25.

### 2026-09-13 · PRD, `01`, `06` and `12` revised for C-1, C-2 and delta 1
- **PRD:** NFR-14 counts quota units. SM-21 and SM-22 report per mode and record the provider tier. New NFR-15 makes the Console API-first. §7.3's trace link is conditional on tracing being built (DLT-06).
- **`01`:** the Console serves and consumes `/api/v1`, and D-02 is reworded. Two model modes and quota windows (§8). The published port is loopback-only, with a fifth CI assertion (DLT-01). MCP is labelled as not in v1 (DLT-06).
- **`06`:** P-9's residual is higher on a future mobile client.
- **`12`:** the frontier provider is named in the processors table. Pilot-gate condition 14: a paid non-training tier before real data.

### 2026-09-13 · C-1 and C-2 recorded as ADRs, and costed by Hat B
- **ADR-0036 (C-1):**
  - Every v1 dependency is free, and each has a named upgrade path.
  - Default frontier provider: the Gemini API free tier.
  - A zero-cost local-only mode, the default for anyone cloning the repo. Eval figures state their mode.
  - The spend ledger and breaker count quota units: provider, tier, requests and windows.
  - Free-tier limits live in configuration, never in documents (O-23).
- **ADR-0037 (C-1, privacy):** free-tier terms are acceptable only because synthetic data and typed-facts-only prompts hold together. Real data requires a paid non-training tier (pilot-gate condition 14), backed by a `tier: free` start-up guard.
- **ADR-0038 (C-2):**
  - One deployable serves the UI and `/api/v1`, and D-02 is reworded.
  - The Console consumes its own API, enforced by a lint rule and a contract test.
  - A token model with refresh and per-device revocation is specified now, with cookies only in v1.
  - The SR-03 draft rule applies to every client.
  - P-9's residual grows on mobile, and the ADR says so.
- **Hat B (feasibility review §11):**
  - 1.5 days against the owner's 1.25. The difference is quota-window handling.
  - Nothing extra lands before day 12, so the checkpoint holds.
  - Contingency drops to 0.5 day, and no build-if-time item fits.
  - Recommends OD-4: a command-line client of `/api/v1` instead of upload pages, which wins back 0.5 day and demonstrates API-first (O-24).
- Amendment links added to ADR-0016, 0020, 0025, 0026, 0028 and 0030.

### 2026-09-13 · Owner decisions on O-17 and delta 1, two new constraints
- **O-17:** Phase 2 measures SM-17, SM-19, SM-20, and SM-18 for dispute findings only. The owner confirmed that "SM-16, SM-17, SM-19" used pre-DOC-01 numbers and was wrong. SM-16 stays with OCR (DF-06). ADR-0032 now quotes the owner's words, and ADR-0024 and ADR-0032 are decided, not proposed.
- **O-19, all six delta-1 findings decided and recorded as ADR amendments:**
  - DLT-01, ADR-0028: loopback-only Console port in v1.
  - DLT-02, ADR-0029 and ADR-0021: approved seller text leaves drafting prompts, and is rendered afterwards as a slot. Only typed facts and templates reach a hosted model. This also closes SR-05.
  - DLT-03, ADR-0027: the checkpoint wrapper raises before querying if no tenant is bound, and an empty checkpoint for an existing case is an error.
  - DLT-04, ADR-0032: Phase 2 absorbs its suites. Features shrink, never suites.
  - DLT-05, ADR-0029: export cells are escaped, plus a doc-lint rule for control symmetry (O-20).
  - DLT-06: folded into the next revision of `01` and the PRD (O-22).
- **Scope:** `04-RAG-DESIGN.md` moves to Phase 2's first day (ADR-0035 amended).
- **New constraints:** C-1, everything on free tiers in v1 with a documented upgrade path. C-2, API-first, so a mobile client can be built later without a backend rewrite. Both are to be recorded as ADRs, with Hat B's costing, before `02` and `03` (O-21).

### 2026-09-13 · Repo made public
- At the owner's request, so the doc set can be shared with another AI by link.
- Scanned every version of every file first: no secrets, keys or tokens, no private or college email, and all 15 commits use the GitHub noreply address. The only personal detail is the mention of the SETU company org and `N073` in this file and ADR-0019, which the owner accepted by asking for a public repo.
- Checked afterwards: the repo page, the file page and the raw file all open without a GitHub login.

### 2026-09-12 · Hat C delta 1 over step 4. STOP.
- Reviewed ADR-0020 to 0035 (`a213c8b`) and the revised documents (`f9ca1c2`). Each finding was checked against its ADR **and** the document text a builder reads.
- **Every finding is resolved or explicitly scheduled:** 7 closed, 4 closed for v1 with a scheduled residual, 1 accepted (SR-17), 1 open pending a build-if-time item (SR-10), 5 to verify in documents not yet written (SR-04, SR-06, SR-15, SR-18, SEC-01), and 1 partly closed (SR-05).
- **6 new findings, recorded in the review:**
  - **DLT-02 (High):** ADR-0021 and ADR-0029 contradict each other. Approved seller free text may enter drafting prompts, but v1's redaction is slot-level only, so a name in that text can reach the hosted model. Must be resolved before `03`.
  - **DLT-03 (Medium):** a checkpoint read with the tenant setting missing returns zero rows, which LangGraph treats as a new thread, so a case silently restarts.
  - **DLT-01 (Medium):** v1 without MFA still publishes the login beyond loopback.
  - **DLT-04 (Medium):** Phase 2 names no security suites, and has no slack to add them.
  - **DLT-05 (Low):** formula injection in generated spreadsheets.
  - **DLT-06 (Low):** stale MCP and trace-link text in `01` and the PRD.
- **Correction, logged here:** the dash check run before `f9ca1c2` never read the files, because zsh does not word-split `$VAR`. It was re-run file by file during delta 1: 43 files read, and the only dashes are in the verbatim `BRIEF.md`, so `f9ca1c2` was clean.
- 🛑 STOP. Waiting for the owner's review of step 4 and delta 1.

### 2026-09-12 · Step 4: Hat A's revision applied
- The owner approved the feasibility review on 2026-09-11, including its §3 disagreement (seven safety invariants, not four), and decided OD-1 to OD-3. OD-1 came with a correction: RAG and the dispute agent return in a bounded 8-day Phase 2 after v1.
- **16 ADRs** (`a213c8b`) record every `01` D-decision and `06` S-decision. They resolve SR-01 to SR-18 and SEC-01, apply the feasibility review's §8 list, and add Phase 2 (ADR-0032), FB-01 (ADR-0033) and FB-02 (ADR-0034). ADR-0035 records the owner's sequence change. ADR-0010 to 0018 carry amendment links.
- **Documents revised to match, each with a revision history:**
  - the PRD: new FR-APR-3 and FR-ING-6, rule A8, v1 status tables, SM-18 rescoped, the SM-21 caveat, VERIFY tags due by build day 21
  - `01`: the migrate job and database roles, fail-closed redaction, hosted calls limited to drafting in v1, Prometheus out of v1, open questions closed or deferred
  - `06`: where each S-decision landed and its v1 status (§4.1), the v1 suite ranking (§5.1), v1 without MFA as a residual
  - the feasibility review: the owner's decisions, FB-01, FB-02
  - `12` is unchanged, as the owner directed for DF-09 and DF-11
- **Correction, logged here:** ADR-0024 and ADR-0032 as committed in `a213c8b` described SM-18's return for dispute findings as "the owner's direction on 2026-09-12". After a session restart, that direction is not in the record: the answer to Hat A's Phase 2 metric question was lost to a tool error. Both ADRs now call it a proposal pending confirmation (O-17).

### 2026-09-11 · IMPL-FEASIBILITY-REVIEW written (Hat B, step 3). STOP.
- **As written, the doc set is 65 to 70 developer-days.** 28 days holds about 20 planned. The committed v1 is 22.0 days at midpoint: 2 over, stated openly, with a scope rule at the day-12 checkpoint.
- **v1 is:** the four claims (isolation, citation gate, no send path, spend cap), the statutory engine as the gate's carrier (L4 notice, template-only), and two AI slices, Reconciliation and Conversation.
- **Security suites:** Hat B agrees with the owner's four non-negotiables and adds ST-01 (stub half), ST-05 and ST-07 (role matrix), plus ST-08 if Razorpay ships. The reason is that the PRD states seven safety invariants, not four. The additions cost 1.25 days. Nine suites are committed.
- **Authentication:** full S-02 costs 4.5 days, and v1 needs 1.0. MFA, step-up and recovery codes become DF-01.
- **Cuts are labelled, not dropped:** DF-01 to DF-16 are in `12` §9.2. Build-if-time items have declared fallbacks (DF-17 to DF-22, and SK-06). SK-01 to SK-06 are skipped, with reasons.
- **Owner decisions (O-15):** OD-1 defer the RAG layer, which conflicts with the brief. OD-2 move Razorpay to build-if-time. OD-3 move evidence packets to build-if-time.
- Checked before commit: no dashes, every B, DF, SK, OD, ST, S, T, SR, O, requirement, D-, A-Q and ADR reference resolves, and the Gantt chart parses.
- 🛑 STOP (brief step 3).

### 2026-09-11 · Hat C approved. Owner finding SEC-01 recorded.
- The owner approved `06`, `12` and the review, and confirmed SR-01 to SR-04 as genuine. SR-02 found a send path that would have defeated ADR-0011 in production.
- **SEC-01 (Medium, the owner's finding, missed by Hat C):** the migration role and the runtime role must be separate. Recorded in the bug register and in the review. It lands in `02` (O-14).
- `06`, `12` and the review are now Frozen for step 3 review. Next: Hat B's feasibility review (step 3).

### 2026-09-11 · SEC-REVIEW-ARCH written. Hat C step 2 complete. STOP.
- 18 findings against the PRD, `01` and the ADRs: 1 Critical, 6 High, 7 Medium, 4 Low. The Critical and High findings (SR-01 to SR-07) must be resolved before the build (O-12).
- **SR-01 (Critical):** the gate's regulated tokens did not cover payment identifiers, URLs, emails or phones. A compromised buyer mailbox could get an attacker's bank details echoed into an approved reply. Fix: S-04.
- SR-02 and SR-03 are the two new send paths, through Razorpay and MCP. SR-04 is the checkpoint schema sitting outside tenant isolation. SR-05 is pseudonymisation failing open. SR-06 is stored injection into drafting prompts. SR-07 is file-borne attacks.
- Checked before commit: no dashes; the inline VERIFY set in `06`, `12` and the review equals `06` Appendix A; every T, S, ST, SR, DC and P reference resolves; every ADR, requirement, D- and A-Q reference resolves; the `06` diagram parses.
- 🛑 STOP (brief step 2). Next, in the owner's order: Hat B's feasibility review (step 3), or Hat A's `02` to `05`.

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

One row per ADR. The ADR file holds the full reasoning, and this row holds the one-line why. ADR-0001 to 0019 were accepted at checkpoint 1 on 2026-09-11 (`75cc43c`). ADR-0020 to 0035 were accepted at step 4 on 2026-09-12. ADR-0010 to 0018 were amended at step 4, as each file's header shows. The files are in [`adr/`](adr/).

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

**Step 4 decisions (2026-09-12)**

| ADR | Decision | Why, in one line | Resolves or amends |
|---|---|---|---|
| [0020](adr/ADR-0020-v1-runtime-network-and-region.md) | v1 runs on Compose on a developer machine. Only workers have egress. ap-south-1 for the target. | Records D-02, D-03, D-06, S-03, S-12 and S-14 after F-02 and F-03 | Resolves SR-13, SR-17, SR-18, A-Q2, A-Q3, A-Q7 |
| [0021](adr/ADR-0021-model-gateway-fail-closed.md) | One model gateway. Pseudonymisation fails closed. In v1, hosted calls are drafting only. | SR-05 and SR-16, and nothing in v1 needs free-text hosted calls | Amends 0016. Resolves D-01, D-04, D-09, S-05. |
| [0022](adr/ADR-0022-durable-execution-details.md) | Database clock, advisory lock with a dirty flag, outbox relay inside the scheduler | Records D-05 and D-07 | Amends 0013 |
| [0023](adr/ADR-0023-audit-log-integrity.md) | Append-only audit in v1. The hash chain and external anchor are build-if-time. | SR-10: a chain inside the database proves nothing on its own | Resolves D-08, S-10 |
| [0024](adr/ADR-0024-citation-gate-extended.md) | Payment identifiers join the regulated tokens. Echo check. Statutory artifacts are template-only. | SR-01 (Critical): payment redirection through approved prose | Amends 0010. Narrows SK-01; SM-18 moves to Phase 2. |
| [0025](adr/ADR-0025-no-third-party-send-paths.md) | No third-party send paths. Razorpay hardened and build-if-time. | SR-02: Razorpay could message the buyer itself | Amends 0011. Resolves SR-08, SR-12, OD-2. |
| [0026](adr/ADR-0026-mcp-deferred-with-constraints.md) | MCP deferred from v1, with its constraints fixed | SR-03 and F-01: MCP versus P9, and unapproved drafts | Closes A-Q8 and F-01 |
| [0027](adr/ADR-0027-isolation-checkpoints-and-db-roles.md) | Checkpoints under tenant isolation. Separate migration and runtime roles. | SR-04, and the owner's SEC-01 | Amends 0012 and 0014 |
| [0028](adr/ADR-0028-authentication-v1-minimum.md) | v1 authentication is passwords, sessions and RBAC. MFA waits for the pilot gate. | Full S-02 costs 4.5 days that protect nothing in v1 | Resolves S-02, SR-09, DF-01 |
| [0029](adr/ADR-0029-untrusted-content-boundaries.md) | Drafting sees only typed facts. User text is untrusted. Cheap file controls. No fetching. | SR-06, SR-07, SR-11 and SR-15 | Resolves S-08, S-09 |
| [0030](adr/ADR-0030-tracing-alerts-real-data-gate.md) | The spend ledger is the cost truth. Langfuse is build-if-time. Alerts are deferred. The real-data gate lives in the schema. | SR-14: a flag alone relies on memory | Amends 0015. Resolves S-11. |
| [0031](adr/ADR-0031-v1-scope.md) | The v1 scope of record: B1 to B15, build-if-time order, DF and SK lists, PRD changes, FR-APR-3 | The doc set was three times the window | Resolves OD-3 and O-10 |
| [0032](adr/ADR-0032-phase-2-dispute-agent-and-rag.md) | Phase 2: the dispute agent with its RAG layer, 8 days, hard stop | Retrieval engineering is deferred for v1, not dropped | The owner's correction to OD-1 |
| [0033](adr/ADR-0033-statute-verification-by-day-21.md) | V01 to V24 verified by build day 21 | FB-01: the old trigger never fired | Amends the feasibility review §7 |
| [0034](adr/ADR-0034-sm21-reported-with-its-cause.md) | SM-21 is always reported with its cause | FB-02: a side effect of scope must not read as optimisation | Amends 0016 and 0017 |
| [0035](adr/ADR-0035-hat-c-before-02-to-05.md) | Hat C ran before Hat A's 02 to 05, with a second delta pass | Records the owner's sequence change, which until now lived only in this file | Amends 0018 |

**Owner constraints and decisions (2026-09-13)**

| ADR | Decision | Why, in one line | Resolves or amends |
|---|---|---|---|
| [0036](adr/ADR-0036-free-tiers-local-only-mode-quota-units.md) | Free tiers in v1 with upgrade paths, a zero-cost local-only mode, and quota units | C-1: v1 must cost nothing, and anyone cloning the repo must be able to run it | Amends 0016, 0030 |
| [0037](adr/ADR-0037-free-tier-model-terms-and-privacy.md) | Free-tier model terms are acceptable only while prompts carry typed facts and templates on synthetic data | C-1: the privacy trade-off must be explicit, with an end condition | Adds `12` §9.1 condition 14 |
| [0038](adr/ADR-0038-api-first.md) | API-first: one deployable serves the UI and `/api/v1`, the token model is specified, and the draft rule applies to every client | C-2: a mobile client later without a backend rewrite | Amends 0020 (D-02), 0025, 0026, 0028 |
| [0039](adr/ADR-0039-demo-driver.md) | A demo driver runs the v1 story through `/api/v1`, replacing the upload pages | OD-4: a one-command demo is worth more than an upload page, and it proves C-2 | Amends the feasibility review's B13 |
| [0040](adr/ADR-0040-drafting-must-earn-its-hosted-path.md) | SM-25 and a blind drafting comparison decide whether N-11's hosted path earns its place | FB-03: the last hosted AI component had never faced the load-bearing test | Amends PRD §6.2 and §10.2, `02`, `03` |

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
| SEC-01 | 2026-09-11, owner review of Hat C | Medium | `06` §3.2 requires a runtime database role that owns nothing and has no `BYPASSRLS`, while `01` §4 has the Console run Prisma migrations, which need DDL rights. One connection string cannot do both. | Two documents written from different angles both implied the role split, and neither stated it. Hat C checked controls against threats, not against the configuration `01` gives the same component. | To be specified in `02-DATA-MODEL.md`: a migration role run as a separate job, and a runtime role under RLS, with each component's role stated (O-14) | The `06` §3.2 startup check, plus a test that the runtime role cannot run DDL | Open until `02` |
| DOC-04 | 2026-09-13, Hat A while applying ADR-0040 | Low | `03`'s conventions sent readers to §4.3, §12 and §13 for nodes outside v1, decisions and open questions. They are §4.2, §11 and §12. | Sections were renumbered while `03` was written. The lint accepted a reference whenever its major section existed, so a reference to a wrong but existing section passed, and `03` was approved with the defect. | The three references corrected, and a strict scan run across every document (exact headings, list continuations inherited, and decision and open-question pointers checked against heading titles) | The section-reference rule in `11` (O-20) | Fixed |

Symptom and root cause are separate columns on purpose. In one LMS batch, every root cause turned out to differ from the reported symptom.

*2026-09-11 note (F-05):* DOC-01's symptom cites "SM-23" in the old numbering on purpose, because it describes the defect. That metric is now SM-06. Every other SM reference in every document resolves to the metric it means (see the log).

*2026-09-11 note (F-01):* decided by Hat C in `06` S-01, option (a), tightened. MCP returns pseudonymised structured records only, never raw text and never the body of an unapproved draft. The ADR lands at step 4 (O-13).

*2026-09-12 note (step 4):* F-01 is closed by ADR-0026. F-04's missing PRD requirement is now FR-APR-3 (ADR-0031). SEC-01 is decided in ADR-0027, and its table-level grants land in `02` (O-14).

## Plans

**Documentation phase (now):** the checkpoint tracker in Part 1. After Hat C's delta 1: Hat A's 02 to 05, Hat C's delta 2 (ADR-0035), Hat B's 07 to 11, then README and INDEX.

**Build phase:** 28 days and one developer, which is 24 working days: 20 planned and 4 of contingency. The feasibility review §6 sets the order. All four claim suites pass by elapsed day 12, and the statute text is loaded in Phase 4 (days 21 to 24). `09-BUILD-PLAN.md` formalises this. C-1 and C-2 add 1.5 days, all after day 12. OD-4, taken as a demo driver (ADR-0039), wins back 0.5 day, so contingency is 1.0 day: enough for build-if-time item 1 and nothing more (feasibility review §11).

**Phase 2 (ADR-0032):** a named follow-on of 8 working days after v1 closes, with a hard stop. Its only feature is the dispute agent (DF-09) with the RAG layer that serves it (DF-11), and its trajectory suite lands with the feature. `04-RAG-DESIGN.md` is written on its first day. Its security suites, extensions of ST-01 and ST-03, sit inside the 8 days. When it runs short, features shrink, never suites. Whatever is unfinished at day 8 goes back to `12` §9.2 as a pilot-gate decision.

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

**Deferred from v1 by the feasibility review (DF).** DF-01 to DF-16 are pilot-gate items in `12` §9.2. Each is marked Required or needing a decision. Build-if-time items not built by day 24 join them as DF-17 to DF-22.

**Skipped (SK).** Not built, not in the pilot gate, and back only through a new ADR:
- SK-01 the G6 entailment check and SM-18
- SK-02 model prose in statutory artifacts
- SK-03 the review-queue digest
- SK-04 AWS Network Firewall
- SK-05 a staging environment
- SK-06 real-data detection, if it is not built

The reasons are in the feasibility review §5.4.

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
- **Check each control against the configuration other documents give the same component.** `06` required a runtime role that owns nothing, while `01` had the same Console run migrations (SEC-01).
- **Add up the scope against the calendar at every checkpoint, not once at the end.** The doc set grew to three times the build window before anyone totalled it (feasibility review §2).
- **Never record a decision as the owner's unless the owner's words are in the record.** A lost tool result left two ADRs crediting the owner with a metric choice that nobody can now show (O-17).
- **A check that cannot fail is not a check.** The step-4 dash check reported PASS without reading a single file. After a lint run, confirm it actually read the files it names, for example by printing a count.
- **Decisions written in the same pass can contradict each other.** ADR-0021 and ADR-0029 were written minutes apart and disagree about hosted drafting inputs (DLT-02). A delta review has to cross-read them.
- **A control that fails closed is only safe if the layer above treats the closed result as a failure.** RLS returning zero rows would have looked like a brand-new case to LangGraph (DLT-03, the owner's lesson).
- **A lesson written down is not yet a check.** F-04 taught "apply a control to every instance of its pattern", and DLT-05 repeated the mistake anyway: formula evaluation was banned on import, and nobody applied it on export. The fix is a doc-lint rule (O-20).
- **Remove an input rather than filter it, when the input needs no model.** Approved seller text needed no rewriting, so taking it out of drafting prompts made ADR-0021's invariant absolute, with no detector to maintain (DLT-02).
- **Apply the load-bearing test to every AI component, including the last one.** The project cut AI from three components and never asked whether its only hosted node earned its place (FB-03, the owner's finding).
- **A lint that falls back to a coarser match finds missing references, never wrong ones.** `03` pointed at existing but wrong sections, and passed (DOC-04).
- **A model that fits the GPU budget on paper can still fail.** The budget shares memory with macOS, Docker and everything else running. 16.8 GB of weights looked fine against a 19.07 GB budget, and still never sat fully on the GPU. Set the pass rule first, then measure (O-06).
