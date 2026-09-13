# Chukta: Data Classification

| | |
|---|---|
| **Purpose** | Classifies every kind of data Chukta holds. Sets how each class is stored, accessed, sent to models, logged, kept and erased. Also sets the rules for synthetic data in v1, and the gate that must pass before any real data. |
| **Intended reader** | The developer building v1, Hat A and Hat B, and whoever approves a pilot. |
| **Status** | Approved by the owner 2026-09-11. Revised 2026-09-13 for ADR-0036 and ADR-0037 (§7, and §9.1 condition 14). Later changes go through an ADR. |
| **Author hat** | Hat C, Security and Compliance Engineer |
| **Last updated** | 2026-09-13 |
| **Related** | [`06-SECURITY-THREAT-MODEL.md`](06-SECURITY-THREAT-MODEL.md) for threats, controls and the VERIFY register from V25 onward. [`01-ARCHITECTURE.md`](01-ARCHITECTURE.md) §7 for where data lives. |

### Conventions

- **Classes** are DC-0 to DC-5 (§1). Every item also carries a **trust label** (§2). The class decides who may see an item. The trust label decides how it may reach a prompt.
- `[VERIFY Vnn]` tags are listed in Appendix A of `06`. This document adds no register of its own, so there is only one list to keep in step.
- Retention periods marked "(default)" are configuration defaults proposed here, not targets. **Statutory retention periods are never stated from memory.** They are tagged and resolved against primary sources.

---

## 1. Classes

| Class | Name | Definition | Examples |
|---|---|---|---|
| DC-0 | Public | Published by a government, or by us for anyone | Statute text, bank-rate history, product documentation |
| DC-1 | Internal | Operational data with no tenant content | Metrics without payloads, configuration, code, synthetic eval data |
| DC-2 | Tenant confidential | A tenant's business data that is not about an identifiable individual | Invoices, POs, amounts, document numbers, companies' names and GSTINs, statutory computations, ledger lines between companies |
| DC-3 | Personal | Any data about an individual who can be identified from it [VERIFY V25: DPDP Act 2023 s.2, "personal data"] | AP contacts' names, emails, phones and designations. Names and signatures on scans. Message bodies. Sole proprietors' trade names. Users' account details. Call notes. Embeddings of DC-3 text. |
| DC-4 | Sensitive personal and financial | DC-3 data whose exposure causes financial harm, and data the older SPDI Rules treat as sensitive [VERIFY V38: IT Act 2000 s.43A and SPDI Rules 2011] | Bank account numbers, IFSC with an account number, individuals' PAN, whole bank statements, individuals' UPI IDs |
| DC-5 | Secret | Anything that grants access, or can re-identify | Password hashes, TOTP seeds, recovery codes, session and MCP tokens, API keys, webhook secrets, the pseudonym map |

**Rules of thumb**

- **A record takes the highest class of anything in it.** A message body is DC-3 even when it is mostly business talk.
- **Sole proprietors blur DC-2 and DC-3.** The business name may be the person's name, and a GSTIN embeds the holder's PAN [VERIFY V42: GSTIN format]. By default, a counterparty is treated as an individual (DC-3) unless the tenant marks it as a company.
- **Embeddings of DC-3 text are DC-3.** An embedding can leak the text it came from.

---

## 2. Trust label

Class and trust are separate. A buyer's email is DC-3 and untrusted. A statutory computation is DC-2 and system.

| Label | Applies to | May reach a prompt as |
|---|---|---|
| `untrusted` | Anything from a counterparty, any upload, and any free text a user types that can reach a prompt (call notes, CA notes, edits) | Delimited data only. Never as instructions, and never into a drafting prompt (`06` §3.1). |
| `system` | Computed by our code, approved artifacts, the verified statutory corpus, templates | Slot values and instructions |

User-typed text is `untrusted` for prompts because users can be careless or compromised, even though they are authenticated (SR-15).

---

## 3. Inventory

| Item | Class | Trust | Stored in | Hosted model | Retention | Erasure |
|---|---|---|---|---|---|---|
| User account: name, email, role | DC-3 | system | Postgres | Never | Life of the account, plus 90 days (default) | On account deletion. Audit rows keep a pseudonymous user ID. |
| Password hash, TOTP seed, recovery codes | DC-5 | system | Postgres, encrypted column | Never | Life of the account | With the account |
| Session and MCP tokens | DC-5 | system | Postgres | Never | Until expiry or revocation | On expiry |
| Tenant profile: name, GSTIN, Udyam number, classification history | DC-2, or DC-3 for a proprietor | system once confirmed | Postgres | Never needed | Life of the tenant, plus the legal retention for tax records [VERIFY V40], [VERIFY V41] | On tenant closure, after legal retention |
| Customers (buyers) | DC-2, or DC-3 for a proprietor | untrusted until confirmed | Postgres | Pseudonymised when DC-3 | Legal retention, because they sit on invoices | After legal retention |
| Buyer contacts: name, email, phone, designation | DC-3 | untrusted | Postgres | Pseudonymised | While the account has open receivables, plus 1 year (default) | Erasure tooling (§6) |
| Invoices, POs, credit notes | DC-2 | untrusted until matched | Postgres | Amounts and references only | Legal retention [VERIFY V40], [VERIFY V41] | After legal retention |
| Scans and PDFs: challan, GRN, POD, e-way bill | DC-3 (names, signatures, phones) | untrusted | Object store, raw bucket | Never as images (`01` D-04). OCR text is pseudonymised if sent. | Legal retention for invoice-supporting documents [VERIFY V40] | After legal retention |
| Counterparty ledger statements | DC-2, with DC-3 in narrations | untrusted | Raw file in the object store, parsed rows in Postgres | Narrations pseudonymised | Raw file: 90 days after the BCS is confirmed (default). Parsed rows: with the ledger. | Scheduled deletion |
| Bank statements | DC-4 | untrusted | Raw file in the object store, only matched credit lines in Postgres | Never raw. Matched lines pseudonymised. | Raw file: deleted 7 days after parsing (default). Matched lines: with the payments. | Scheduled deletion |
| Inbound email: body and attachments | DC-3 | untrusted | MIME in the object store, body in Postgres | Local model only, or pseudonymised | While the case is open, plus 1 year (default) | Erasure tooling |
| WhatsApp chat exports | DC-3 | untrusted | Raw file deleted after import. Only the selected chat and date window are kept, as inbound messages. | Local model only, or pseudonymised | Raw: deleted the same day. Imported messages: as inbound email. | Scheduled deletion and erasure tooling |
| Call notes, CA notes, other user free text | DC-3 | untrusted for prompts | Postgres | Pseudonymised | With the case | Erasure tooling |
| Drafts pending approval | DC-2, DC-3 | system slots with model-written prose | Postgres, object store | Generated there, and never sent anywhere else | With the case | With the case |
| Approved artifacts and approvals | DC-2, DC-3 | system | Object store (immutable), Postgres | Never | Legal retention, as business correspondence [VERIFY V40] | After legal retention |
| Audit events | DC-2, with user IDs | system | Postgres, append-only | Never | The longest of the legal log-retention minimums [VERIFY V35], [VERIFY V39], and never shorter than the life of the artifacts they cover | Not erasable. User IDs are pseudonymous. |
| Spend ledger: task, tier, tokens, cost | DC-1 | system | Postgres | Not applicable | 2 years (default) | Scheduled deletion |
| Prompts and outputs | DC-3 | mixed | Not stored except as traces | Not applicable | Traces are kept in pseudonymised form, at the shortest retention Langfuse allows (`06` S-05) | Langfuse retention setting |
| Pseudonym map | DC-5 | system | Postgres, encrypted column, per tenant | Never | Until the case closes, plus the retention of its inbound messages | With the case |
| Chunks and embeddings | Class of the source (DC-3 when from DC-3 text) | untrusted (source) | Postgres, pgvector | Chunks pseudonymised when sent | With the source item | Deleted with the source, in the same transaction |
| Razorpay events | DC-3 (payer contact fields) | untrusted until verified | Postgres | Never | With the payments | With the payments |
| Statutory corpus, bank rates | DC-0 | system once verified | Postgres | Yes, it is public | Kept, versioned | Not applicable |
| Eval datasets G, H, A and R | DC-1 (synthetic) | untrusted, treated as buyer content | The repo, from the build phase | Yes, it is synthetic | Kept | Not applicable |

---

## 4. Handling rules by class

| Rule | DC-0 | DC-1 | DC-2 | DC-3 | DC-4 | DC-5 |
|---|---|---|---|---|---|---|
| Encryption at rest | Disk | Disk | Disk | Disk | Disk, plus column encryption for account numbers and PAN | Column encryption or hashing |
| Who can read | Anyone | Developers | Tenant users, by role | Tenant users, by role | Owner and accountant | Code paths only |
| Hosted model | Yes | Yes | Yes | Pseudonymised only | Never, except as pseudonymised tokens | Never |
| Logs | Yes | Yes | IDs only | IDs only | Never | Never |
| Traces | Yes | Yes | Yes, in pseudonymised form | Pseudonymised only | Pseudonymised tokens only | Never |
| MCP responses | Yes | No | Yes, as structured records | Pseudonymised only (`06` S-01) | Never | Never |
| Export by a user | Yes | No | Owner, with step-up | Owner, with step-up and an audit event | Owner, with step-up and an audit event | Never |
| Backups | Yes | Yes | Encrypted | Encrypted | Encrypted | Encrypted, with keys held separately |

"Disk" means full-disk encryption on the developer's machine in v1, and storage encryption on RDS and S3 in the production target.

---

## 5. Minimisation at import

Uploads often carry far more personal data than a case needs (T-18). Minimise at the door.

| Source | What we keep | What we drop | When |
|---|---|---|---|
| WhatsApp export | Messages from the chat and date window the user selects | Every other chat, and media not attached to a case | At import. The raw file is deleted the same day. |
| Bank statement | Credit lines that match, or might match, a customer | Debits, unrelated credits, other payers' details | At parsing. The raw file is deleted 7 days later (default). |
| Email | The message and the attachments relevant to the case | Quoted threads outside the case. Signature blocks beyond name, role and phone. Tracking pixels and remote images. | At ingestion |
| Scans | The fields we extract, plus the image as evidence | Nothing. The image is the evidence. | Kept, with access by role |
| Counterparty ledger | Rows and narrations | Nothing else in the file is needed | The raw file is deleted 90 days after the BCS is confirmed (default) |

---

## 6. Retention and erasure

- **Two duties pull against each other.** Personal data should be erased once its purpose is served [VERIFY V29: DPDP Act 2023 s.8(7)]. Tax and GST law require business records to be kept for set periods [VERIFY V40: CGST Act 2017 s.36], [VERIFY V41: Income-tax Act 1961 s.44AA and Rule 6F, and their 2025 equivalents]. So Chukta keeps the business record, and erases or pseudonymises its personal parts once their purpose is served. Each item under a legal hold records the hold and its reason.
- **Erasure tooling.** Given an identifier (a name, email or phone number), the tool:
  1. Finds every record that holds it: contacts, messages, notes, scans, embeddings, the pseudonym map and trace references.
  2. Shows the list to the owner.
  3. Erases what is not under a legal hold, and pseudonymises what is.
  4. Writes an audit event.

  Requests reach the tenant, as Data Fiduciary, and Chukta provides the tool (`06` §3.7).
- **Backups** expire on their own rotation. An erased item survives in older backups until they expire. A restore re-applies the erasure log before the system opens.
- **Langfuse** is set to its shortest retention, and holds only pseudonymised data.

---

## 7. Residency and processors

| Processor | What it receives | Location | Notes |
|---|---|---|---|
| The developer's machine (v1) | Everything, synthetic only | India | Full-disk encryption required |
| AWS (production target only) | Everything, encrypted | ap-south-1, Mumbai | Not used in v1 |
| Hosted frontier model provider: the Gemini API free tier in v1 (ADR-0036) | Typed facts and templates only, with pseudonymised identifiers (ADR-0021, ADR-0037). The provider's current terms are confirmed under PROJECT_CONTEXT O-23. | Probably outside India | Cross-border transfer is allowed unless the destination is a restricted country [VERIFY V31: DPDP Act 2023 s.16]. Zero-retention terms are checked before a pilot. |
| Langfuse Cloud | Pseudonymised traces | The region chosen at signup | ADR-0015. The real-data gate applies. |
| Razorpay | The payment link's amount and description, and payment events | India | Test mode only in v1. No buyer contact fields are sent (`06` S-06). |
| The user's MCP client and its model | Pseudonymised structured records (`06` S-01) | Unknown, chosen by the user | The least controlled route. It is disabled for real tenants until the pilot gate passes. |

**Answer to `01` A-Q7: ap-south-1 for the production target.** Keeping primary data in India limits the cross-border question to pseudonymised model traffic. It also fits CERT-In's direction on keeping logs within Indian jurisdiction [VERIFY V39: CERT-In Directions of 28 April 2022].

---

## 8. Synthetic data rules for v1

- **No real person's data anywhere**, including the human-written H set. Contributors write fictional messages about fictional people.
- **Identifiers are made impossible on purpose:**
  - GSTINs and PANs use a deliberately wrong check character, so they can never match a real registration.
  - Emails use only reserved example domains: `example.com`, `example.org`, `example.net`, or the `.example` suffix.
  - Phone numbers use a range that cannot be a real Indian mobile number [VERIFY V43: DoT National Numbering Plan].
  - Bank account numbers carry a fixed synthetic marker prefix, so tooling can tell synthetic data at a glance.
- **Real-looking data is flagged at ingestion (`06` S-11).** Any of these raises a review item saying "this looks like real data": a GSTIN or PAN with a valid check character, a phone number that could be a real mobile, or an email on a non-example domain. While ADR-0015's real-data gate is in force, such an item can be deleted but never accepted.
- **Every tenant is created with `synthetic = true`.** Creating any other kind fails while the gate is in force.

---

## 9. Pilot-readiness gate

No real tenant and no real personal data until every condition below is met and recorded in `PROJECT_CONTEXT.md`. Meeting them is what lifts ADR-0015's real-data gate.

### 9.1 Conditions

| # | Condition | Why | Owner |
|---|---|---|---|
| 1 | Chukta's role confirmed: Data Processor for tenant data, Data Fiduciary for its own users [VERIFY V37] | Decides who owes which duty | Owner, with counsel |
| 2 | A data processing agreement template for tenants | A processor acts under contract [VERIFY V29] | Owner, with counsel |
| 3 | A published sub-processor list: model provider, tracing, Razorpay, AWS | Tenants must know where their data goes | Owner |
| 4 | Hosted-model and tracing terms confirmed: no training on our data, minimal or zero retention | P9 assumes it | Owner |
| 5 | A notice and lawful-basis pack that tenants can use for their counterparties' contacts [VERIFY V27], [VERIFY V28] | Tenants carry the fiduciary duty | Owner, with counsel |
| 6 | Erasure tooling built and tested (§6) | Data principals' rights [VERIFY V30] | Developer |
| 7 | A breach runbook covering both the DPDP and CERT-In timelines [VERIFY V34], [VERIFY V39] | Two regimes, different clocks | Owner and developer |
| 8 | Logs kept for the legal minimums, within India [VERIFY V35], [VERIFY V39] | Required retention | Developer |
| 9 | MFA enforced, step-up live, RBAC tests passing (`06` S-02, ST-07) | Account takeover is rated High | Developer |
| 10 | Every ST suite in `06` §5 passing, and SM-01 to SM-06 at their targets | These are the safety invariants | Developer |
| 11 | ADR-0015's replacement decided: Langfuse Cloud with masking and a processing agreement, or self-hosting | Traces carry data | Owner, Hat A |
| 12 | An external penetration test of the deployed target | Nothing has attacked it yet | Owner |
| 13 | The commencement status of the DPDP Act and Rules checked on the day [VERIFY V33] | Obligations are phasing in | Owner |
| 14 | Every hosted model and tracing provider on a paid tier whose terms exclude training on submitted content, with minimal or zero retention. No free tier with real data. | ADR-0037: free-tier terms are acceptable only while data is synthetic and prompts carry typed facts and templates | Owner |

### 9.2 Deferred from v1

These rows were written by Hat B's feasibility review (`reviews/IMPL-FEASIBILITY-REVIEW.md`), at the owner's direction, so that nothing cut from v1 is lost. Each row is a pilot-gate item. A **Required** row must be built before any real data. A **Decide** row needs an explicit decision before the pilot: either build it, or re-label it as skipped through an ADR.

| ID | Deferred item | Came from | Before the pilot |
|---|---|---|---|
| DF-01 | TOTP MFA, step-up and recovery codes, and ST-07's MFA and step-up cases | `06` S-02 | **Required** (condition 9) |
| DF-02 | Malware scanning, and the full file-borne suite ST-10 | `06` S-08 | **Required** |
| DF-03 | Minimisation tooling at import, and ST-15 | §5 | **Required** before any real export or bank statement is imported |
| DF-04 | Erasure tooling | §6 | **Required** (condition 6) |
| DF-05 | Prometheus metrics and alerts, including approval-queue and review-queue ageing | `01` §11 | **Required** |
| DF-06 | OCR, plus classification and extraction of scanned documents | PRD FR-ING-4, `01` A-Q6 | Decide. Real evidence arrives as scans. |
| DF-07 | Text-PDF and scanned-PDF ledgers and bank statements | PRD FR-ING-2 | Decide |
| DF-08 | Live inbound mail and WhatsApp export import | PRD FR-ING-3, `01` A-Q1 | Decide. A pilot cannot run on fixtures. WhatsApp import brings DF-03 with it. |
| DF-09 | The dispute agent, its trajectory evals, and SM-19 and SM-20 | PRD FR-DSP-1 | Decide |
| DF-10 | The analyst: DSO, CEI, forecast and chase list | PRD FR-ANL-1, FR-ANL-2 | Decide |
| DF-11 | The RAG layer: hybrid retrieval, reranking, three corpora, multi-hop, and SM-17. The entity graph stays in v1. | Brief §2, `04` | Decide. **This conflicts with the brief, so the owner decides now.** |
| DF-12 | The MCP endpoint. Until it exists, `06` S-01 option (c) applies. | PRD FR-INT-3 | Decide. If it is built, S-01's constraints and ST-06's MCP cases are required. |
| DF-13 | Devanagari inbound support and its eval set | PRD §3.1 | Decide |
| DF-14 | Hosted calls that carry free text (the frontier explanation of residuals), and the name-finding pseudonymisation pass they need | `01` §8, `06` S-05 | **Required together.** No hosted free-text call happens before the full pipeline exists. |
| DF-15 | The L3 interest-statement template | PRD §5.8 | Decide. L4 carries the gate claim in v1. |
| DF-16 | A mapping step for non-standard register layouts | PRD FR-ING-1 | Decide. The Tally adapter may replace it. |

- **Build-if-time items** that are not built by the end of v1 join this table then, as DF-17 to DF-22. The feasibility review §5.3 pre-declares which ID each one takes.
- **Skipped items (SK-nn) are deliberately not here.** They are not pilot-gate items, and they come back only through an ADR. They are listed in the feasibility review §5.4 and in `PROJECT_CONTEXT.md`.

---

## 10. Revision history

| Date | Change | Why |
|---|---|---|
| 2026-09-11 | First version | Hat C, step 2 |
| 2026-09-11 | Approved by the owner, and frozen for step 3 review | Owner's review |
| 2026-09-11 | §9 split into 9.1 (conditions) and 9.2 (items deferred from v1, DF-01 to DF-16) | The feasibility review, at the owner's direction: deferred items must go into the pilot gate, not be dropped |
| 2026-09-13 | §7 names the v1 frontier provider and what it receives. §9.1 gains condition 14: a paid non-training tier before real data. | ADR-0036, ADR-0037 |
