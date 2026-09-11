# ADR-0001: The 43B(h) rule is a year-end deferral (behaviour B-1)

| | |
|---|---|
| **Purpose** | Records how Chukta treats the 43B(h) rule, why, and what it rules out. |
| **Intended reader** | Anyone changing the statutory engine, the escalation ladder or notice templates. |
| **Doc status** | In Review |
| **Decision status** | Accepted, 2026-09-11 (checkpoint 1) |
| **Related** | PRD §1.1, §5.8, FR-STA-4, SM-08; ADR-0002, ADR-0003 |

## Context

The brief says a buyer who pays a micro or small supplier late "loses the tax deduction for that expense in that year". As we read s.43B(h), a sum paid after the MSMED s.15 time limit is deductible in the year it is actually paid [VERIFY V01: Income-tax Act 1961 s.43B(h)]. The proviso that rescues other clauses when payment is made before the return due date does not reach clause (h) [VERIFY V02: s.43B first proviso]. So the deduction moves; it is not lost. A buyer who pays late but inside the same financial year is unaffected. The effect arises only when the statutory due date has passed and the sum is still unpaid at 31 March [VERIFY V05]. From 1 April 2026 the Income-tax Act 2025 carries the equivalent provision [VERIFY V03], and sums that straddle that date need a transition rule [VERIFY V04].

s.16 interest behaves differently. It accrues from the day after the statutory due date, all year [VERIFY V11: MSMED Act 2006 s.16].

## Decision

- Model the two levers as named behaviour **B-1, the year-end lever window** (PRD §5.8). s.16 interest is the year-round lever. The 43B(h) rule is the January to March lever.
- No outbound draft mentions the 43B(h) rule between 1 April and 31 December.
- From 1 January to 31 March, invoices that qualify for the 43B(h) rule and are due on or before 31 March get the window ladder: a dated year-end note from L2, and shorter intervals.
- Wording is fixed by template: "the deduction moves to the tax year of payment". The word "lost" never appears.
- From 1 April, 43B(h) wording is withdrawn for invoices of the year that just closed.
- Invoices that straddle 1 April 2026 stay undetermined for the 43B(h) rule until V04 is verified.

## Alternatives rejected

| Alternative | Why rejected |
|---|---|
| Mention the 43B(h) rule whenever an invoice is past its statutory due date | False for most of the year. A wrong legal claim under the owner's name damages both credibility and the relationship. |
| Drop the 43B(h) rule and rely on s.16 alone | Throws away a real Q4 lever for the qualifying subset |
| Let a model decide when the rule applies | It is a date comparison. Code does it exactly, and a model can only add error. |

## Consequences

- The engine takes "today" as an explicit input, so every B-1 rule is testable with frozen dates (SM-08).
- Ladder configuration has two shapes, and tenant intervals must allow L2 and L3 before 31 March.
- Notice templates are versioned by Act (1961 and 2025).
- Business metrics must control for Q4 seasonality, because B-1 changes Q4 behaviour by design (BO-01).
