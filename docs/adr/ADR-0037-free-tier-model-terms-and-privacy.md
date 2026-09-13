# ADR-0037: Free-tier model terms are acceptable in v1 only because nothing but typed facts and templates reach a hosted model

| | |
|---|---|
| **Purpose** | Makes the privacy trade-off of using a free hosted model tier explicit, and sets the condition under which it must end. |
| **Intended reader** | Anyone configuring a hosted provider, and whoever approves a pilot. |
| **Doc status** | In Review |
| **Decision status** | Accepted, 2026-09-13. Records the owner's constraint C-1, privacy part. |
| **Resolves** | Constraint C-1, privacy decision (2026-09-13) |
| **Amends** | `12` §9.1: adds condition 14 |
| **Related** | ADR-0021, ADR-0029, ADR-0030, ADR-0036; P9 in `01` §1; PROJECT_CONTEXT O-23 |

## Context

Free tiers of hosted model APIs generally reserve the right to use submitted content to improve the provider's products, which can include training and human review. The current terms of the provider named in ADR-0036 are confirmed under O-23, not asserted here. The owner required this trade-off to be written down, not left implicit.

## Decision

- **Using a free tier is acceptable in v1, for two reasons together:**
  1. v1 holds synthetic data only (ADR-0030).
  2. After DLT-02, the only thing that reaches a hosted model is a drafting prompt built from typed facts and templates (ADR-0021, ADR-0029). No free text of any origin, no raw counterparty content, no scans. Identifiers inside slots are pseudonymised, and the pre-send scan still blocks anything that slips.
- **Neither reason alone would be enough.** Synthetic data with free-text prompts would still train the provider on the product's prompt design and behaviour. Typed-facts-only prompts built from real data would still hand real business figures to a provider that may train on them.
- **Pilot-gate condition 14 (`12` §9.1):** real data requires a paid tier whose terms exclude training on submitted content, and set minimal or zero retention. The same applies to tracing (condition 11).
- **An enforcing guard, not just a rule:** a hosted provider configured as `tier: free` refuses to start if any tenant has `synthetic = false`. That state cannot exist while ADR-0030's check constraint stands, so the guard is a second lock for the day the constraint is lifted.
- **Local-only mode (ADR-0036) needs none of this:** a tenant can run with no hosted calls at all.

## Alternatives rejected

| Alternative | Why rejected |
|---|---|
| Leave the trade-off implicit | The owner ruled it must be explicit. An implicit trade-off would be forgotten at the pilot. |
| No hosted model in v1 at all | The brief routes drafting to a frontier model, and local-only mode already covers the zero-external-API case |
| Accept free-tier terms for real data too, relying on pseudonymisation | Typed facts still carry real amounts, dates and business relationships |

## Consequences

- `12` §9.1 gains condition 14 (next revision).
- The processors table in `12` §7 names the provider and its tier.
- O-23 checks the provider's current free-tier and paid-tier terms before any claim about them appears in a document.
