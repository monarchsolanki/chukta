# ADR-0011: No send capability in v1. The send adapter is specified, and Razorpay test mode is in scope.

| | |
|---|---|
| **Purpose** | Records why Chukta v1 cannot transmit messages, what replaces sending, and what payment integration stays in scope. |
| **Intended reader** | Anyone working on approval, outbound artifacts, payments, egress rules or the threat model. |
| **Doc status** | In Review |
| **Decision status** | Accepted, 2026-09-11 (checkpoint 1). Modified: the send adapter is specified in 07. Razorpay test-mode links and webhooks are in scope. Amended at step 4 by ADR-0025 (2026-09-12). |
| **Related** | PRD §1.3, §3.1, §5.7, §7.1, NG1, NG7, FR-INT-2, FR-PAY-1, FR-PAY-2, SM-03 |

## Context

The brief rules out autonomous sending and requires human approval of every outbound artifact. It does not say who transmits after approval. Having the system send would need the WhatsApp Business API (business verification, template approval) and email deliverability work. It would also put a send path right next to the approval gate. That path is the main way the gate could be bypassed.

## Decision

- **v1 has no code path that transmits a message to a counterparty.** Approval produces the final artifact plus a `wa.me` or `mailto` link. A human sends it from their own account and marks it sent (PRD §7.1, A4).
- **An outbound send adapter is specified but not implemented** in `07-API-CONTRACTS.md`, together with the stronger gate it would need. The bytes sent must be proven identical to the bytes approved. Adding it later should be a small implementation, not a redesign.
- **Razorpay test mode is in scope.** Creating a payment link sends nothing to the buyer. Test-mode webhooks give a closed loop that can be demonstrated: link generated, test payment, signed webhook, HMAC verified, deduplicated, ledger updated, invoice state advanced.

## Alternatives rejected

| Alternative | Why rejected |
|---|---|
| The system sends after approval | Needs a gate proving the sent content equals the approved content, per-channel provider integrations and WhatsApp template approval. Adds the main bypass risk. |
| No payment integration at all | Loses a demonstrable closed loop and real webhook handling |
| Live Razorpay keys | Needs a real merchant account and real money. Pilot only (NG7). |

## Consequences

- SM-03 can be checked statically. The codebase has no messaging client, and an egress allowlist permits only model providers, the Razorpay test API and tracing.
- "Sent" is a human assertion, so BO-08 depends on people marking artifacts sent.
- Payment links follow the order in PRD §7.1 A6: approve, create the link, render the final artifact.
