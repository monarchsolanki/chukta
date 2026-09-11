# ADR-0004: s.16 interest uses versioned method parameters and an effective-dated bank-rate table

| | |
|---|---|
| **Purpose** | Records how s.16 interest is computed, and why every method choice is explicit. |
| **Intended reader** | Anyone working on the statutory engine, reference test cases, or statutory artifacts. |
| **Doc status** | In Review |
| **Decision status** | Accepted, 2026-09-11 (checkpoint 1) |
| **Related** | PRD §5.3, FR-STA-3, NFR-09, SM-07; ADR-0002, ADR-0005 |

## Context

s.16 sets compound interest with monthly rests at three times the RBI bank rate [VERIFY V11: MSMED Act 2006 s.16]. As far as we know, the text does not fix four method choices [VERIFY V12: s.16 and MSEFC practice]:
1. Which rate applies when the bank rate changes mid-period.
2. Where the monthly rests are anchored: calendar months, or monthly from the due date.
3. How a part month is counted.
4. Whether TDS that the buyer deducted and deposited reduces the principal.

The bank rate itself changes over time [VERIFY V13: RBI Bank Rate history].

## Decision

- Interest is a pure function of: principal schedule, start date, end date, a **method profile**, and the bank-rate table.
- A method profile (for example `s16-method-v1`) names every choice above. Each choice stays tagged VERIFY until the owner or their CA confirms it.
- The bank rate lives in an effective-dated table loaded from RBI sources (PRD Appendix B.3). It is never hard-coded.
- Every output prints the profile, each rate applied with its effective date, and the rest schedule, so a CA can reproduce the figure by hand.

## Alternatives rejected

| Alternative | Why rejected |
|---|---|
| Hard-code one interpretation | Hides a judgment call, and cannot adapt when a CA or MSEFC practice differs |
| Let a model compute interest | Arithmetic that cannot be reproduced is worthless in a notice |
| Simple interest as an approximation | Contradicts the statute as we read it |

## Consequences

- Every reference test case states its profile (SM-07).
- Changing the default profile needs a new ADR.
- Recomputing a past case uses the profile version and rates recorded at the time (NFR-09).
- The approver sees the method, not just the number (PRD §7.3).
