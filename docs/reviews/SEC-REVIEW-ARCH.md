# Chukta: Security Review of Hat A's Documents

| | |
|---|---|
| **Purpose** | Hat C's findings against Hat A's documents. Each finding has a severity, an attack or failure scenario, and a concrete mitigation. Hat A resolves them at step 4. |
| **Intended reader** | Hat A for the step 4 revision, Hat B for feasibility and tests, and the owner |
| **Status** | Approved by the owner 2026-09-11, with SR-01 to SR-04 confirmed genuine. Delta 1 run 2026-09-12. The owner decided all six delta-1 findings on 2026-09-13 (below). |
| **Author hat** | Hat C, Security and Compliance Engineer |
| **Last updated** | 2026-09-13 |
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

### Delta 1: Hat A's step 4 revision (2026-09-12)

| | |
|---|---|
| **Reviewed** | ADR-0020 to 0035 at `a213c8b`, and the revised PRD, `01`, `06` and feasibility review at `f9ca1c2` |
| **Method** | Each finding was checked twice: against its resolving ADR, and against the revised document text a builder would actually read. Then the step-4 changes were probed for new problems, including contradictions between ADRs written in the same pass. That is the check SEC-01 showed this review had skipped. |
| **Verdict** | Step 4 resolves every finding or schedules it explicitly. **No new Critical finding.** One new High (DLT-02): two step-4 ADRs disagree about what a hosted drafting prompt may contain. It must be resolved before `03` fixes the node input contracts. |

#### Resolution of SR-01 to SR-18 and SEC-01

| Finding | Resolved by | Evidence in the documents | Verdict |
|---|---|---|---|
| SR-01 | ADR-0024 | PRD §7.1 A2 lists the new token classes. `06` G3 scans for them. | **Closed** |
| SR-02 | ADR-0025 | PRD FR-PAY-1 requires Razorpay's notifications and reminders off, with no buyer contact details | **Closed.** The loop is build-if-time, and its controls ship with it or not at all. |
| SR-03 | ADR-0026 | PRD FR-INT-3 forbids raw text and unapproved draft bodies. MCP is not built in v1. | **Closed** |
| SR-04 | ADR-0027 | `01` §7 names `tenant:case` threads. PRD NFR-02 covers checkpoints. | **Resolved, to verify in `02`** (delta 2). See DLT-03. |
| SR-05 | ADR-0021 | PRD NFR-03 and `01` §5.4 and §8 require a blocking pre-send scan and fail closed | **Partly closed.** DLT-02 finds a gap in the v1 claim. |
| SR-06 | ADR-0029 | No document yet. Node input contracts belong in `03`, retrieval profiles in `04`. | **Decided, to verify in `03` and `04`** (delta 2) |
| SR-07 | ADR-0029 | PRD FR-ING-6. Malware scanning is DF-02 in `12` §9.2. | **Closed for v1.** The residual is Required before the pilot. |
| SR-08 | ADR-0025 | PRD FR-PAY-2 finds the payment by our own link ID and cross-checks it | **Closed** |
| SR-09 | ADR-0028 | `01` §9 names the seeding CLI as the recovery path | **Closed for v1.** Full recovery arrives with DF-01. See DLT-01. |
| SR-10 | ADR-0023 | `01` §9 marks the hash chain and anchor as build-if-time item 4 | **Open** until item 4 is built, or DF-20 at the pilot gate |
| SR-11 | ADR-0029 | v1 imports no WhatsApp exports or PDF statements. Minimisation tooling is DF-03. | **Closed for v1.** The residual is Required before real imports. |
| SR-12 | ADR-0025 | PRD §7.1 A8 | **Closed.** The screenshot residual is accepted. |
| SR-13 | ADR-0020, ADR-0025 | `01` §10.1: the tunnel exists only with Razorpay, and routes only webhook paths | **Closed** |
| SR-14 | ADR-0030 | A schema check constraint holds `synthetic = true` | **Closed for v1.** Detection is build-if-time item 5, otherwise SK-06. |
| SR-15 | ADR-0029 | `12` §2 already labels user-typed text untrusted. Its use in prompts is fixed in `03`. | **Decided, to verify in `03`** (delta 2) |
| SR-16 | ADR-0021 | `01` §5.4 traces the pseudonymised payload | **Closed** |
| SR-17 | ADR-0020 | The CI assertion is named in ADR-0020 | **Accepted,** with the assertion to be placed in `11` |
| SR-18 | ADR-0020 | NTP and log retention are named for the production target | **Decided, to verify in `10`** |
| SEC-01 | ADR-0027 | `01` §4 adds the migrate job. `01` §7 names both roles and which component uses which. | **Resolved.** The table-by-table grants are verified in `02` (delta 2). |

**Tally:** 7 closed, 4 closed for v1 with a scheduled residual, 1 accepted, 1 open pending a build-if-time item, 5 to verify in documents not yet written, and 1 partly closed.

#### New findings from delta 1

| ID | Severity | Where | Finding and scenario | Mitigation | Lands in |
|---|---|---|---|---|---|
| DLT-01 | Medium | ADR-0028, `01` §10.1 | ADR-0028 accepts no MFA in v1 because v1 is "one machine". But nothing binds nginx's published port to loopback. On shared Wi-Fi, any device on the network reaches a password-only login. The seeded demo users also invite fixed passwords committed in seed code. | Publish nginx on `127.0.0.1` only, and point the tunnel at loopback. The seeding CLI generates random passwords, shows them once and never writes them to the repo. ST-09 asserts the loopback binding, and ST-13 checks seed files for password literals. | `10`, `11` |
| DLT-02 | **High** | ADR-0021 versus ADR-0029 | ADR-0021 says v1's hosted calls are drafting prompts built from slots, so slot-level pseudonymisation is complete without the name-finding pass. ADR-0029 lets drafting prompts include **seller-authored text that has already been approved**. That is free text. A prior approved message such as "as discussed with Suresh ji on his mobile" contains a person's name that is in no slot. It reaches the hosted model, and the pre-send scan, which catches patterns and known contacts, can miss a name it has never seen. v1 exposure is nil because the data is synthetic, but the design as written fails its own claim at the pilot. | Until DF-14's name-finding pass exists, hosted drafting prompts receive typed facts and templates only. Approved seller text may feed drafting on the local SLM, never a hosted call. Amend ADR-0021 or ADR-0029 with one sentence, and add an ST-05 case: an approved message holding an unslotted name never appears in a hosted payload. | ADR amendment, then `03` |
| DLT-03 | Medium | ADR-0027 | RLS on the checkpoint tables fails closed by returning zero rows. LangGraph manages its own connections, so if the tenant setting is missing on a checkpoint read, the library sees "no checkpoint" and starts the thread fresh. The case silently restarts: interrupts and approval context are lost, and drafts can be produced twice. Failing closed as empty is indistinguishable from a new case. | Checkpointer calls run inside the data-access wrapper's transaction, with the tenant set. The wrapper treats "no checkpoint" for a case whose record shows earlier runs as an error that moves the case to NeedsHuman, not as a fresh start. ST-03 adds a case with the setting absent. | `02`, `03` |
| DLT-04 | Medium | ADR-0032 | Phase 2 puts retrieved untrusted spans into frontier prompts, and adds a new retrieval API. ADR-0032 names no security suites, and its 8 days have no slack, so the suites would be the first thing cut. | Amend ADR-0032: before the first dispute run, ST-03 covers the retrieval API, and ST-01's stored-injection cases run against the dispute agent on a real model. If they do not fit in 8 days, the dispute agent runs on the local SLM only and hosted dispute calls wait. The suites are not cut. | ADR-0032 amendment, `05`, `09` |
| DLT-05 | Low | ADR-0029, generated files | Any CSV or XLSX Chukta generates from counterparty data can carry a narration that starts with `=`, `+`, `-` or `@`. When the owner opens the file, the spreadsheet runs it as a formula. | Escape such leading characters in every generated spreadsheet cell. Add a case to the ST-10 smoke test. | `07`, `11` |
| DLT-06 | Low | `01` §3, PRD §7.3 | Stale v1 text. The `01` §3 diagram still draws MCP clients and an MCP endpoint, which are deferred (ADR-0026). PRD §7.3 promises the approver a trace link, but tracing is build-if-time (ADR-0030). A builder reading either would build the wrong thing. | Label MCP as not in v1 in the diagram. Change §7.3 to "a trace link, once tracing is built". | Next revision of `01` and the PRD |

#### The owner's decisions on delta 1 (2026-09-13)

| Finding | Owner's decision | Recorded in | Status |
|---|---|---|---|
| DLT-01 | Publish the Console port on `127.0.0.1` only in the v1 Compose file. Nothing in v1 needs remote access, so deferring MFA becomes a non-issue in v1. The S-12 tunnel still routes webhook paths. | ADR-0028 amendment | Decided. Lands in `01` §10.1 and `10`. |
| DLT-02 | Remove the input instead of filtering it. Approved seller free text leaves drafting prompts entirely, and is rendered into the artifact after generation as a slot sourced from its approved record. ADR-0021's invariant becomes absolute: only typed facts and templates reach a hosted model. No DF-14 work and no detector for this. | ADR-0029 and ADR-0021 amendments | Decided. **This also closes SR-05.** Verified in `03`'s node contracts (delta 2). |
| DLT-03 | The checkpoint wrapper asserts the tenant is bound and raises before querying. A zero-row checkpoint for an existing case is an error, never a new thread. Both join ST-03. | ADR-0027 amendment | Decided. Lands in `02` and `03`. |
| DLT-04 | Phase 2 keeps its 8-day hard stop and absorbs its suites, as extensions of ST-01 and ST-03 budgeted at 0.5 day. When Phase 2 runs short, features shrink, never suites. | ADR-0032 amendment | Decided |
| DLT-05 | Prefix leading `=`, `+`, `-` and `@` in every exported cell. **Root cause, in the owner's framing:** S-08 banned formula evaluation on import, and nobody applied it on export. That is F-04's lesson repeating, which means the lesson is written down but is not yet a check. A doc-lint rule in `11` will make it one. | ADR-0029 amendment; `11` doc-lint rule (PROJECT_CONTEXT O-20) | Decided. The lint rule lands in `11`. |
| DLT-06 | Fold into the next revision of `01` and the PRD | PROJECT_CONTEXT O-22 | Decided |

**Updated tally for SR-05:** closed by DLT-02's resolution. That makes 8 closed, 4 closed for v1 with a scheduled residual, 1 accepted, 1 open pending a build-if-time item, and 5 to verify in documents not yet written.

### Delta 2: `02`, `03` and `05` (ADR-0035)

*Not yet run.* It verifies SR-04, SR-06, SR-15 and SEC-01 in `02` and `03`, and DLT-02 and DLT-03 where they land. `04` moved to Phase 2's first day, and gets its own review then.
