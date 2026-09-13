# ADR-0031: v1 scope: built, build-if-time, deferred and skipped

| | |
|---|---|
| **Purpose** | Records what v1 is, after the feasibility review, and the changes this makes to frozen documents. |
| **Intended reader** | Everyone. This is the scope of record for v1. |
| **Doc status** | In Review |
| **Decision status** | Accepted, 2026-09-12 (step 4) |
| **Resolves** | The feasibility review's verdict and its change list (feasibility review §8). The owner's decisions OD-1 (v1 part) and OD-3 (2026-09-11). The disposition of `01` A-Q1, A-Q5 and A-Q6. PROJECT_CONTEXT O-10. |
| **Related** | `reviews/IMPL-FEASIBILITY-REVIEW.md`; `12` §9.2; ADR-0023 to ADR-0030, ADR-0032 |

## Context

As written, the doc set was 65 to 70 developer-days of work. 28 days holds about 20 planned days. The feasibility review cut v1 to build items B1 to B15 (22.0 days), labelled everything else, and the owner approved it on 2026-09-11.

## Decision

- **v1 is B1 to B15, in 24 working days:** 20 planned and 4 of contingency. The four claim suites pass by elapsed day 12, and the day-12 scope rule applies (feasibility review §6).
- **Build-if-time, in order:**
  1. Evidence packets (OD-3)
  2. The Razorpay loop (ADR-0025)
  3. Langfuse (ADR-0030)
  4. Audit anchoring (ADR-0023)
  5. Real-data detection (ADR-0030)
  6. The manual acceptance-reset action
  7. The real-model security cases

  Each has the fallback declared in the feasibility review §5.3.
- **Deferred:** DF-01 to DF-16 in `12` §9.2, unchanged. DF-09 and DF-11 are scheduled for Phase 2 (ADR-0032). The rest wait for the pilot gate's decision.
- **Skipped:** SK-01 to SK-06 (feasibility review §5.4). SK-01 is narrowed to the G6 check on statutory prose (ADR-0024).
- **PRD goals:** G1 and G7 are build-if-time. G5 is Phase 2. G6 is deferred (DF-10).
- **PRD requirements:**
  - FR-EVD-1 to 3, FR-PAY-1 to 2 and FR-DSP-2 are build-if-time.
  - FR-DSP-1 is Phase 2.
  - FR-ANL-1 to 2 and FR-INT-3 are deferred.
  - FR-ING-1 to 4 shrink as described in the feasibility review's B9.
  - FR-STA-5 is template-only (ADR-0024).
  - FR-HQ-5 loses its digest (SK-03).
  - **New, FR-APR-3:** the Console flags any artifact that has waited for approval past a configured age, and the flag names the stalled case. This closes O-10. The matching alert waits for DF-05.
- **PRD metrics:**
  - SM-15 is not measured unless evidence packets are built. SM-16 waits for DF-06.
  - SM-17 to SM-20 are measured in Phase 2 (ADR-0032).
  - SM-21 is always reported with its cause (ADR-0034).
- **PRD §3.1:** Devanagari inbound support is deferred (DF-13).
- **Architecture open questions:** A-Q1 (mail provider) is deferred with DF-08, and A-Q6 (OCR engine) with DF-06. A-Q5 was settled by F-03: there is no GPU host in v1.

## Alternatives rejected

| Alternative | Why rejected |
|---|---|
| Keep the full scope | Three times the window |
| Cut the claims' test suites to make room for features | A gate with no mutation suite is an untested claim |

## Consequences

- The PRD, `01`, `06` and the feasibility review carry step-4 revision notes that point here.
- `09-BUILD-PLAN.md` formalises the sequence at step 5.
