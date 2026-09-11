# ADR-0003: Eligibility is decided per invoice, and Statutory is a subset feature

| | |
|---|---|
| **Purpose** | Records how statutory eligibility is determined and how the feature is positioned in the product. |
| **Intended reader** | Anyone working on eligibility, classification data, statutory UI or product scope. |
| **Doc status** | In Review |
| **Decision status** | Accepted, 2026-09-11 (checkpoint 1). Modified: Evidence and Reconciliation positioned as the primary product. |
| **Related** | PRD §1.3, §3.2, §3.3, FR-STA-1, BO-10; ADR-0001, ADR-0002 |

## Context

The statutory levers apply to a narrower group than "registered MSMEs":
- Only micro and small enterprises, on the relevant date [VERIFY V17: MSMED Act s.7 and the classification notification], [VERIFY V21: reclassification].
- The supplier must be registered [VERIFY V18: s.2(n), s.8], possibly before the contract [VERIFY V19: case law].
- Traders on Udyam appear to be outside the delayed-payment chapter [VERIFY V20].
- The 43B(h) rule affects only buyers who claim the expense as a business deduction [VERIFY V22], while s.16 applies to any buyer [VERIFY V23].
- There is no public Udyam verification API we can rely on [VERIFY V24].

## Decision

- Eligibility is decided **per invoice**, against conditions E1 to E6 (PRD §3.2). Each lever is gated separately.
- There are three outcomes: qualifying (and for which lever), not qualifying (with reason codes), and undetermined (with the missing input).
- Classification is stored with effective dates. It is extracted from the uploaded Udyam certificate, and a human confirms it. There is no portal automation.
- **Positioning:** Evidence and Reconciliation are the primary product. Statutory is a high-value feature for a qualifying subset. For everyone else it shows "not applicable" with the reason codes.

## Alternatives rejected

| Alternative | Why rejected |
|---|---|
| One eligibility flag per seller | Wrong when classification changes mid-year, or when one seller both trades and manufactures |
| Assume every Udyam-registered seller qualifies | Overstates the lever and produces invalid notices |
| Scrape the Udyam portal | Out of scope (NG5). Fragile, with terms-of-use risk. |
| Make Statutory the headline feature | Most target sellers may not qualify. Leading with it would promise value we cannot deliver to them. |

## Consequences

- The data model needs classification history, an activity type per supply, and a buyer tax profile.
- The UI shows reason codes, not just a yes or no.
- BO-10 measures how large the qualifying subset really is in a pilot.
- Ladder steps L3 and L4 are reachable only by qualifying invoices.
