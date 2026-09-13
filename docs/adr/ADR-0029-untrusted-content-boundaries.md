# ADR-0029: Untrusted content boundaries: drafting inputs, user text, files, fetching and minimisation

| | |
|---|---|
| **Purpose** | Records what content may reach which prompt, and how v1 handles files, links and over-collection. |
| **Intended reader** | Anyone writing prompts, node input contracts, parsers or importers. |
| **Doc status** | In Review |
| **Decision status** | Accepted, 2026-09-12 (step 4). Amended 2026-09-13 by the owner's decisions on DLT-02 and DLT-05. |
| **Resolves** | `06` S-08 and S-09; SR-06, SR-07, SR-11 and SR-15; DF-02 and DF-03 |
| **Related** | `06` §3.1; `12` §2, §5; ADR-0024 |

## Context

Hat C found four gaps:
- Drafting prompts had no content boundary (SR-06).
- User-typed text was not labelled untrusted (SR-15).
- Files had no safety handling (SR-07).
- Imports over-collected personal data (SR-11).

The feasibility review kept the cheap file controls and deferred malware scanning and the minimisation tooling.

## Decision

- **Drafting prompts** receive only typed facts and templates. Untrusted spans never enter them (SR-06). **Approved seller free text never enters them either** (amended 2026-09-13, DLT-02). It is the seller's own words and needs no model to rewrite it, so it is rendered into the artifact after generation, as a slot sourced from its approved record. Dispute investigation, in Phase 2, may read untrusted spans, but it outputs only a JSON assessment that code validates. `03` fixes each node's input contract, and `04` defines the retrieval profiles that enforce it.
- **User-typed free text** (call notes, CA notes, approver edits) is labelled `untrusted` for prompts (SR-15, `12` §2).
- **Files in v1:**
  - a type allowlist that rejects macro-enabled formats
  - size limits
  - hardened XML parsing
  - formulas never evaluated
  - parsing in the no-egress worker, under time and memory limits
  - downloads served as attachments with `nosniff`

  Malware scanning and the full ST-10 are deferred as DF-02, Required before the pilot (S-08, SR-07).
- **Nothing is fetched from content:** no link unfurling, no remote images, and HTML email rendered as text (S-09).
- **Minimisation:** v1 imports no WhatsApp exports and no PDF statements (DF-07, DF-08), so most of the over-collection path is absent. The bank CSV import keeps only credit lines that could match a customer. The minimisation tooling and ST-15 are DF-03, Required before any real export or statement is imported (SR-11).
- **Exports** (amended 2026-09-13, DLT-05): in every CSV or XLSX Chukta generates, a cell that starts with `=`, `+`, `-` or `@` is prefixed so the spreadsheet treats it as text. S-08 banned formula evaluation on import, and this is the same control applied to export. The ST-10 smoke test adds an export case.

## Alternatives rejected

| Alternative | Why rejected |
|---|---|
| Malware scanning in v1 | Another dependency, with nothing to catch in a synthetic corpus |
| Trusting user-typed text | Users can be careless or compromised, even though they are authenticated |

## Consequences

- ST-01's stub half covers the drafting boundary.
- The ST-10 smoke test is build-if-time item 7.
