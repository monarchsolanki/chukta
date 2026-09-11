# ADR-0002: The statutory clock starts at acceptance, and disputes can reset it

| | |
|---|---|
| **Purpose** | Records which date starts the statutory payment clock, and how disputes interact with it. |
| **Intended reader** | Anyone working on the statutory engine, the evidence graph or the dispute path. |
| **Doc status** | In Review |
| **Decision status** | Accepted, 2026-09-11 (checkpoint 1) |
| **Related** | PRD §3.2 (E4, E5), §5.5, FR-STA-2, FR-DSP-2; ADR-0001, ADR-0004 |

## Context

The brief gives the 15-day and 45-day periods without saying when they start. As we read the MSMED Act, they run from the day of acceptance or deemed acceptance of the goods or services [VERIFY V06: MSMED Act 2006 s.2(b)], [VERIFY V08: s.15]. A written objection made within 15 days of delivery moves acceptance to the day the objection is removed [VERIFY V07: s.2(b), Explanation]. The invoice date is a different date, usually earlier. We also need a day-count convention [VERIFY V09: General Clauses Act 1897 s.9].

## Decision

- Statutory due date = f(acceptance date, payment terms, day-count convention).
- The acceptance date comes from evidence (signed challan, GRN, POD) or from a resolved objection. A human confirms it before it counts.
- **An unknown acceptance date returns "cannot compute".** The engine never falls back to the invoice date.
- The dispute path emits an acceptance-change flag when it finds a written objection inside 15 days of delivery. Once a human confirms it, the due date recomputes.
- Payment terms extracted by a model need human confirmation too, because they drive a legal deadline (E5).

## Alternatives rejected

| Alternative | Why rejected |
|---|---|
| Use the invoice date | Simple, but systematically wrong. It is usually too early, so notices would go out before the real deadline. |
| Invoice date plus a configurable lag | Still a guess, presented as a statutory date |
| Accept an owner-typed acceptance date with no evidence | Allowed only as a human-queue item that records the evidence gap and the attester. Never the default path. |

## Consequences

- Evidence and Statutory share one acceptance-date field, with provenance: source document, extracted value, confirmer and time.
- Acceptance-date and payment-term confirmations always go to the human queue (FR-HQ).
- "Undetermined" invoices are normal and visible. They create tasks, not guesses.
- The Dispute path feeds the Statutory path, so dispute tests must include acceptance-reset cases.
