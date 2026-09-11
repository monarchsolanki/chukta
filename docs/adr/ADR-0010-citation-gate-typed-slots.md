# ADR-0010: The citation gate uses typed slots and a deterministic scanner

| | |
|---|---|
| **Purpose** | Records how Chukta guarantees that no fact or citation reaches an approver without a bound source. |
| **Intended reader** | Anyone working on templates, drafting, the gate, or its adversarial tests. |
| **Doc status** | In Review |
| **Decision status** | Accepted, 2026-09-11 (checkpoint 1). Amended at step 4 by ADR-0024 (2026-09-12). |
| **Related** | PRD §7.1 (A2, A3), FR-STA-5, SM-01, SM-18; ADR-0005, ADR-0008 |

## Context

The brief says any statutory or factual claim in an outbound artifact must carry a retrieved source span, or the artifact is blocked. But deciding what counts as a "claim" in free text is itself a model judgment. A gate built on it is only as reliable as that model.

## Decision

- Artifacts are built from typed blocks: prose blocks and slots.
- **Regulated tokens** may appear only as rendered slot values bound to a source record, such as a ledger row, document field, engine output or verified provision. Regulated tokens are amounts, dates, day counts, percentages, invoice/PO/challan/GRN/UTR references, section references and provision names.
- After rendering, a **deterministic scanner** finds every regulated token in the final text and checks it against the slot render map. Any token that no slot produced blocks the artifact.
- Amounts spelled out in words ("chaar lakh sattar hazaar", "4.7 lakh") count as regulated tokens too. The slot formatter is the only way to render an amount.
- Cited provisions must be verified and in force on the relevant date (ADR-0005).
- An LLM entailment check runs as a second layer, and its findings are shown to the approver. **A model can add a block, never lift one.**

## Alternatives rejected

| Alternative | Why rejected |
|---|---|
| LLM claim extraction plus span matching | The gate becomes as weak as the extractor |
| Free generation plus a regex check after the fact | Regex finds tokens but cannot bind them to sources. Paraphrased numbers slip past. |
| Fully fixed templates with no model prose | Safest, but cannot follow the thread's language. Kept as the fallback for statutory artifacts. |

## Consequences

- The template engine keeps a provenance map from every slot to its source.
- The scanner needs a pattern library for Indian formats: ₹4,70,000, 4.7L, Rs., INR, lakh and crore in words, many date formats, and "Section 15", "s.15", "sec 15", "u/s 43B(h)".
- The adversarial suite (SM-01) must include spelled-out, Hinglish and Devanagari number forms.
- Any approver edit to prose re-runs the scanner (A3).
