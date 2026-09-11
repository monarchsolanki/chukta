# ADR-0009: One durable case per customer account

| | |
|---|---|
| **Purpose** | Records the unit of durable work in Chukta and why it is the customer account, not the invoice. |
| **Intended reader** | Anyone working on the data model, the orchestrator, message allocation or cost reporting. |
| **Doc status** | In Review |
| **Decision status** | Accepted, 2026-09-11 (checkpoint 1) |
| **Related** | PRD §5, §5.9, FR-CASE-1, FR-CASE-3; ADR-0013 |

## Context

The brief says the orchestrator "owns each overdue invoice as a durable long-lived case". But replies, payments and ledger statements arrive per customer and often cover several invoices ("teeno bill next week"). A lump-sum payment has to be split across invoices. With one case per invoice, every message would be routed to several cases, and several drafts would queue for the same AP contact at once.

## Decision

- A case is (tenant, customer account).
- Inside the case, each invoice has its own state machine (PRD §5.9). Statutory figures stay per invoice.
- At most one outbound draft per account per wake, combining invoices at different steps (FR-CASE-3).
- Inbound messages attach to the account's case first, then are allocated to invoices.

## Alternatives rejected

| Alternative | Why rejected |
|---|---|
| One case per invoice, as briefed | Duplicate routing of each message, parallel drafts to one contact, and allocation logic spread across cases |
| One case per buyer group (parent company) | AP is often centralised across a group, but ledgers and GST registrations are per entity. A group view is deferred. |

## Consequences

- Case to account is 1:1. Invoice state lives inside the case.
- The LangGraph thread ID is the case ID.
- Only one run per case at a time, so concurrency control is needed per case.
- Cost per case (SM-22) is measured per account, not per invoice.
