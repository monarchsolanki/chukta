# ADR-0025: No third-party send paths. Razorpay is hardened and moves to build-if-time.

| | |
|---|---|
| **Purpose** | Extends ADR-0011's no-send guarantee to third parties, hardens the Razorpay loop, and records where it sits in v1. |
| **Intended reader** | Anyone working on payments, webhooks, the approval screen or ST-06 and ST-08. |
| **Doc status** | In Review |
| **Decision status** | Accepted, 2026-09-12 (step 4) |
| **Resolves** | `06` S-06 and S-13; SR-02, SR-08 and SR-12; the owner's decision OD-2 (2026-09-11); the v1 status of PRD FR-PAY-1 and FR-PAY-2 |
| **Amends** | ADR-0011 |
| **Related** | `06` §3.6, §3.8; `reviews/IMPL-FEASIBILITY-REVIEW.md` §5.3; ADR-0020 |

## Context

ADR-0011 said Chukta has no send capability, and the owner kept Razorpay test mode in scope. Hat C then found three problems:
- Razorpay itself can message the buyer (SR-02).
- Webhook handling trusted fields in the payload (SR-08).
- Pending drafts could be copied and sent by hand (SR-12).

The feasibility review moved the loop to build-if-time, and the owner agreed (OD-2).

## Decision

- **ADR-0011 now names third-party send paths and forbids them.** No integration may cause any third party to message a counterparty.
- **Razorpay links** are created with notifications and reminders off, and with no buyer email or phone. The link description comes from an approved slot. A test inspects the outgoing request (S-06, SR-02).
- **Webhooks** find the payment by our own link ID, and take the tenant and invoice from our own record. They check amount, currency and status, and any mismatch goes to the review queue (SR-08).
- **The Razorpay loop is build-if-time item 2** (OD-2). ST-08 ships with it or not at all. If the loop is not built by the end of v1, it becomes DF-18.
- **Pending drafts** carry a watermark. Copy and export are disabled, send links appear only after approval, and every view is audited (S-13, SR-12). The residual, that screenshots cannot be stopped, is accepted.

## Alternatives rejected

| Alternative | Why rejected |
|---|---|
| Trust Razorpay's default settings | Defaults can change, and a default that messages the buyer defeats ADR-0011 |
| Take the tenant from the payload's notes | Anyone who can sign an event can then choose which tenant it credits |
| Keep Razorpay in the committed scope | It proves none of the four claims, and costs 1 day |

## Consequences

- `07` specifies the link-creation payload and the webhook contract.
- ST-06 includes the payload test.
- PRD FR-PAY-1 and FR-PAY-2 become build-if-time.
