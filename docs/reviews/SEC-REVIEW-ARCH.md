# Chukta: Security Review of Hat A's Documents

| | |
|---|---|
| **Purpose** | Hat C's findings against Hat A's documents. Each finding has a severity, an attack or failure scenario, and a concrete mitigation. Hat A resolves them at step 4. |
| **Intended reader** | Hat A for the step 4 revision, Hat B for feasibility and tests, and the owner |
| **Status** | Approved by the owner 2026-09-11, with SR-01 to SR-04 confirmed genuine. Findings stay Open until step 4. |
| **Author hat** | Hat C, Security and Compliance Engineer |
| **Last updated** | 2026-09-11 |
| **Reviewed** | `00-PRD.md` at `078b7d4`, `01-ARCHITECTURE.md` at `762d8b7`, and ADR-0001 to ADR-0019 at `75cc43c`. `02` to `05` are not written yet (PROJECT_CONTEXT O-11). |
| **Method** | The threat model in [`06-SECURITY-THREAT-MODEL.md`](../06-SECURITY-THREAT-MODEL.md), using its severity scale. T-nn, S-nn and ST-nn IDs refer to `06`. DC-n classes refer to [`12-DATA-CLASSIFICATION.md`](../12-DATA-CLASSIFICATION.md). |

---

## Summary

**18 findings: 1 Critical, 6 High, 7 Medium, 4 Low.** The Critical and High findings (SR-01 to SR-07) must be resolved before the build starts. All findings stay Open until Hat A resolves them at step 4. Where a resolution changes the PRD or an ADR, it goes through an ADR.

| ID | Severity | Where | Finding | Mitigation | Lands in |
|---|---|---|---|---|---|
| SR-01 | **Critical** | PRD §7.1 A2, ADR-0010 | The gate's regulated tokens omit URLs, emails, phones, UPI IDs, IFSC codes and account numbers. Injected payment details can pass into approved prose. | S-04 | ADR-0010 amendment, PRD via ADR |
| SR-02 | High | ADR-0011, PRD FR-PAY-1, `01` §5.2 | Razorpay can message the buyer itself: link notifications and reminders are a send path outside the gate | S-06 | ADR-0011 amendment, `07` |
| SR-03 | High | `01` A-Q8, PRD FR-INT-3 | MCP carries raw data past P9, and draft tools could hand unapproved drafts to a client that can send them | S-01, which decides A-Q8 | `07`, ADR at step 4 |
| SR-04 | High | ADR-0012, ADR-0014, `01` §7 | LangGraph checkpoints in `agent_runtime` hold case state with no tenant enforcement | S-07 | ADR-0012 amendment, `02` |
| SR-05 | High | `01` §8, PRD NFR-03, SM-05 | Pseudonymisation has no stated failure behaviour, and SM-05 finds leaks only after they have gone | S-05 | `01` §8 and PRD via ADR |
| SR-06 | High | PRD §6.2, `01` §8 | Nothing says which content drafting prompts may see, so stored injection can reach outbound prose | Drafting sees only typed facts, templates and approved seller text | `03`, `04` |
| SR-07 | High | PRD FR-ING-2, FR-ING-4, `01` §4 | Counterparty files are parsed and served to users with no malware scan, macro policy, parser limits or safe download handling | S-08 | `01` §4 via ADR, `07` |
| SR-08 | Medium | `01` §5.3, PRD FR-PAY-2 | Webhook handling does not bind a payment to our own link record. Tenant or amount could come from the payload. | Resolve by our link ID, then cross-check | `07` |
| SR-09 | Medium | `01` §9 | With no outbound email, account recovery is unspecified, which invites an improvised, insecure reset | S-02 | Decided in `06`, detailed in `07` |
| SR-10 | Medium | `01` D-08 | A hash chain held inside the database can be recomputed by anyone who can write to it | S-10 | `02`, and the ADR that promotes D-08 |
| SR-11 | Medium | PRD §3.1, FR-ING-2, FR-ING-3 | No minimisation rule. WhatsApp exports and bank statements bring in unrelated people's data. | `12` §5 | PRD via ADR, `03` |
| SR-12 | Medium | PRD §7.1 | A pending draft can be copied from the UI and sent by hand before approval | S-13. Residual accepted. | PRD §7.1 via ADR |
| SR-13 | Medium | `01` §10.1 | The dev tunnel for live webhooks is a public surface with no stated hardening | S-12 | `09`, `10` |
| SR-14 | Medium | ADR-0015 | The real-data gate is only a flag. Nothing detects real data entering v1. | S-11 and `12` §8 | ADR-0015 amendment |
| SR-15 | Low | PRD §2, `01` §9 | User-typed free text is not labelled untrusted for prompts | `12` §2 | `03` |
| SR-16 | Low | `01` §5.4 | The gateway re-identifies a reply before tracing it, so trace masking is the only protection | Trace the pseudonymised form (S-05) | `01` §5.4 via ADR |
| SR-17 | Low | `01` §10.1 | nginx sits on a routable network so that it can publish its port | Accepted, with a CI assertion | `11` |
| SR-18 | Low | `01` §6, §10.3 | The production target states neither clock synchronisation nor log retention in India | NTP on every host, retention per `12` | `10` |

### Added after approval

| ID | Severity | Where | Finding | Mitigation | Lands in |
|---|---|---|---|---|---|
| SEC-01 | Medium | `01` §4, `06` §3.2 layer 4 | The database role split is implied but never stated. `06` requires an app role that owns nothing and has no `BYPASSRLS`, enforced by a startup check. But `01` §4 has the Console run Prisma migrations as sole owner, and migrations need DDL rights on the tables. One connection string cannot satisfy both. **Found by the owner. This review missed it.** | Two roles: a **migration role**, which owns the schema and runs migrations as a separate job, and a **runtime role**, which owns nothing and is subject to RLS. State which role each component connects as. | `02-DATA-MODEL.md` (O-14) |

**Why this review missed it:** each control was checked against the threats, not against the configuration another document gives the same component. That check is now a lesson in `PROJECT_CONTEXT.md`.

---

## Findings in detail

### SR-01 (Critical): payment details can be laundered into an approved message

- **Where:** PRD §7.1 A2 and ADR-0010 define regulated tokens as amounts, dates, day counts, percentages, document references and section references.
- **Scenario:** an attacker has compromised the buyer's mailbox (business email compromise). They send a reply saying "our AP team now asks suppliers to quote their bank details as...", with the attacker's account, a UPI ID or a lookalike link. A drafting model echoes it into the seller's reply. Nothing in it is a regulated token, so the gate passes it. The owner skims the prose and approves. The buyer's real AP team sees the seller's own message quoting the wrong account, and pays it.
- **Mitigation (S-04):**
  - Add URLs, email addresses, phone numbers, UPI IDs, IFSC codes and bank account numbers to the regulated tokens.
  - The seller's own bank details render only from a verified-bank-details slot. Changing them needs step-up and is audited.
  - Add an echo check: an outbound draft may not reproduce a span of untrusted content longer than a configured length.
  - Show payment details on the approval screen as a separate, highlighted block.
- **Tests:** ST-02, plus the SM-01 suite, extended with these token types.

### SR-02 (High): Razorpay is a send path

- **Where:** ADR-0011 says Chukta has no send capability. PRD FR-PAY-1 and `01` §5.2 create payment links after approval.
- **Scenario:** Razorpay's Payment Links API can itself send the link to the customer by SMS or email, and can send reminders on its own schedule (confirm the current options in `07`). A link created with the buyer's contact details and notifications on messages the buyer immediately, and again later, in words nobody approved.
- **Mitigation (S-06):**
  - Create links with notifications and reminders explicitly off, and with no customer email or phone.
  - Render the link description from an approved slot.
  - Add a test that inspects the outgoing API request.
  - Amend ADR-0011 to name third-party send paths, and forbid them.

### SR-03 (High): MCP versus P9, and unapproved drafts reaching a client that can send

- **Where:** `01` A-Q8 and PRD FR-INT-3.
- **Scenario:** a user's MCP client, an LLM app with its own email tool, asks Chukta for a draft and then sends it. Separately, raw message bodies returned by MCP flow to a hosted model that Chukta does not control.
- **Mitigation (S-01, which decides A-Q8):** adopt Hat A's option (a), tightened:
  - MCP returns pseudonymised structured records only.
  - It never returns raw message or document text.
  - **It never returns the body of an unapproved draft.** Draft tools add to the approval queue and return an ID and a status.
  - Every call is audited.
  - Tokens are short-lived, scoped to one tenant and revocable.
  - MCP stays off for real tenants until the pilot gate passes (`12` §9).
  - P9 stands, with no carve-out.

### SR-04 (High): checkpoints sit outside tenant isolation

- **Where:** ADR-0012 enforces RLS on the app schema. ADR-0014 moves LangGraph's tables to `agent_runtime`, which has no RLS. `01` §7 lists checkpoints as holding case state.
- **Scenario:** a bug, or a crafted case ID, resumes another tenant's thread. Its checkpoint, holding DC-2 and DC-3 data, loads into a prompt for the wrong tenant.
- **Mitigation (S-07):**
  - Thread IDs become `tenant:case`.
  - A wrapper checks the prefix against the bound tenant on every checkpoint read and write.
  - RLS policies on the `agent_runtime` tables key on that prefix.
  - Checkpoints join the ST-03 isolation suite.

### SR-05 (High): pseudonymisation can fail open, and SM-05 looks too late

- **Where:** `01` §8, PRD NFR-03 and SM-05.
- **Scenario:** the local NER stage times out under load. The gateway sends a dispute thread holding an AP clerk's name to the hosted model. SM-05, which scans logged payloads, notices the next morning.
- **Mitigation (S-05):**
  - Run the SM-05 detectors as a blocking pre-send check.
  - Refuse any hosted call carrying free text when a pseudonymisation stage is unavailable.
  - Record traces before re-identification.
  - Keep SM-05's scan of logs as a second line of defence.

### SR-06 (High): drafting prompts have no stated content boundary

- **Where:** PRD §6.2 ("typed facts and a template") and `01` §8. Neither forbids retrieved untrusted text in a drafting prompt.
- **Scenario:** a hostile line planted in a buyer's email months ago is retrieved as "context" when drafting a reminder on another invoice. The model follows it (T-03).
- **Mitigation:**
  - Drafting prompts receive only typed facts, templates, and seller-authored text that has already been approved.
  - Untrusted spans never enter them.
  - Dispute investigation may read untrusted spans, but outputs only a JSON assessment that code validates.
  - `03` fixes each node's input contract, and `04` defines retrieval profiles that enforce it.

### SR-07 (High): the file-borne attack surface is unaddressed

- **Where:** PRD FR-ING-2 and FR-ING-4, and `01` §4.
- **Scenario:** a PDF crafted to exploit the parser, a zip bomb, an XLSX carrying an XML external entity, or a macro-laden spreadsheet. Or a file that is simply malware, which a user later downloads from Chukta and opens.
- **Mitigation (S-08):**
  - A file-type allowlist. Macro-enabled formats are rejected.
  - A malware scan before storage and before any download.
  - Parsing in the no-egress ingest worker, under CPU, memory and time limits.
  - Hardened XML parsing. Formulas are never evaluated.
  - Downloads served as attachments with `nosniff`.

### SR-08 to SR-18

| ID | Scenario | Mitigation, concretely |
|---|---|---|
| SR-08 | A validly signed event for a different link, or an unexpected amount, credits an invoice because the handler trusted the payload | Look the payment up by our own link ID. Take the tenant and invoice from our record. Check amount, currency and status. Any mismatch goes to the review queue. |
| SR-09 | A locked-out owner and no reset path. Someone edits the database by hand. | Recovery codes at enrolment, and an audited admin CLI reset that needs shell access (S-02) |
| SR-10 | An attacker with database write access alters an approval and recomputes the chain | Anchor the chain head outside the database every day. v1: an append-only file included in backups. Target: an S3 bucket with Object Lock. (S-10) |
| SR-11 | A whole WhatsApp history, including family chats, lands in the tenant's data | Import only the chosen chat and date window. Keep only relevant bank credit lines. Delete raw files on schedule (`12` §5). |
| SR-12 | Staff copy a pending draft into WhatsApp before approval | A watermark. Copy and export disabled. Send links only after approval. Audit of who viewed what. The residual is accepted. (S-13) |
| SR-13 | A tunnel left running exposes the Console UI to the internet | The tunnel routes only webhook paths, via an nginx location allowlist, and runs only during the payment test (S-12) |
| SR-14 | Someone uploads a real customer's ledger "just to try it" | Detect real-looking identifiers at ingestion, and allow only deletion while the gate is in force (S-11, `12` §8) |
| SR-15 | A careless CA note containing instruction-like text reaches a prompt | Label all user-typed free text untrusted for prompts (`12` §2) |
| SR-16 | Trace masking has a bug, and names reach Langfuse | Write the trace from the pseudonymised payload, before re-identification (S-05) |
| SR-17 | nginx is compromised and used to reach the internet | Accepted: nginx holds no secrets and is not on `data`. CI asserts that it cannot reach Postgres and has no upstream outside `app`. |
| SR-18 | Host clocks drift, and logs are kept outside India or for too short a time | NTP on every host (RDS handles its own). Log retention and location per `12` §3 [VERIFY V39]. |

---

## What Hat A's design already does well

- **There is no send path by construction** (ADR-0011). That removes the largest class of approval bypass before any test runs.
- **The slot-based gate** (ADR-0010) makes citation correctness a deterministic property, not a model judgment.
- **The tenant is bound in closures, not passed as an argument** (ADR-0012). A model cannot ask for another tenant.
- **Deterministic engines** compute every figure a notice states (ADR-0007).
- **One model gateway** (D-01) gives a single place to enforce redaction, budgets and tracing. That is why SR-05 is a small fix.
- **Egress comes only from the compute zone,** and after F-03 it is tested on every push (D-03).

---

## Delta reviews

**Delta 1: after Hat A's step 4 revision (ADR-0018).** *Not yet run.* It will check each SR resolution against this document and `06`.

**Delta 2: `02` to `05` (PROJECT_CONTEXT O-11).** *Not yet run.* These were written after this review, because the owner moved Hat C ahead of them.
