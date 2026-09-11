# ADR-0024: The citation gate covers payment identifiers, and statutory artifacts are template-only

| | |
|---|---|
| **Purpose** | Records the extension of the gate after Hat C's one Critical finding, and the decision that statutory artifacts carry no model-written prose. |
| **Intended reader** | Anyone working on the renderer, templates, the gate or ST-04. |
| **Doc status** | In Review |
| **Decision status** | Accepted, 2026-09-12 (step 4) |
| **Resolves** | `06` S-04, SR-01 (Critical), SK-01, SK-02, the model part of PRD FR-STA-5, and SM-18 |
| **Amends** | ADR-0010 |
| **Related** | PRD §7.1 (A2); `06` §3.3; ADR-0005 |

## Context

ADR-0010's regulated tokens covered amounts, dates, day counts, percentages, document references and section references. Hat C found (SR-01, Critical) that the list left out payment identifiers. So an attacker who controls a buyer's mailbox could get their bank details echoed into an approved reply. The feasibility review also found that the G6 entailment check has nothing to check once statutory artifacts are template-only.

## Decision

- **Regulated tokens now also include:** URLs, email addresses, phone numbers, UPI IDs, IFSC codes and bank account numbers. Like the original list, they render only from slots.
- **The seller's own bank details** render only from a verified-bank-details slot, and every change to them is audited. Step-up joins when DF-01 lands.
- **An echo check** blocks any outbound draft that reproduces a span of untrusted content longer than a configured length.
- **The approval screen** shows payment details as a separate, highlighted block.
- **Statutory artifacts are template-only,** with no model-written prose (SK-02). So G6, the entailment check on statutory prose, is skipped (SK-01). It comes back only through an ADR that allows model prose in statutory artifacts.
- **SM-18 no longer applies to statutory artifacts.** It returns in Phase 2, scoped to dispute findings, because their cited spans are chosen by a model (ADR-0032, at the owner's direction on 2026-09-12). SK-01's reasoning, that there is no model prose to check, holds for templates but not for dispute findings.

## Alternatives rejected

| Alternative | Why rejected |
|---|---|
| Keep the original token list | SR-01 stands, and the payment-redirection path stays open |
| Model prose in statutory artifacts, checked by G6 | More moving parts, and no v1 value |

## Consequences

- ST-04 covers the new token classes and ST-02's deterministic cases.
- PRD §7.1 A2 and FR-STA-5 are revised to match.
- SM-18 is removed from v1 reporting.
