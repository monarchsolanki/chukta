# ADR-0039: A demo driver replaces the upload pages (OD-4)

| | |
|---|---|
| **Purpose** | Records the owner's decision on OD-4: a single command drives the whole v1 demo through the public API, with uploads as one of its subcommands, instead of browser upload pages. |
| **Intended reader** | Hat B, writing `09` and `10`. The developer building the Console and the demo. Anyone writing the README. |
| **Doc status** | In Review |
| **Decision status** | Accepted, 2026-09-13. Records the owner's decision on OD-4, which accepted Hat B's recommendation with a change of shape. |
| **Resolves** | OD-4 (PROJECT_CONTEXT O-24) |
| **Amends** | The feasibility review's B13 (Console pages); ADR-0038 (a second client from the first week) |
| **Related** | ADR-0028, ADR-0036, ADR-0038; feasibility review §11.3 |

## Context

Hat B's feasibility review §11.3 proposed replacing B13's upload pages with an upload command that calls `/api/v1`. That would win back 0.5 day of contingency and give the API a second, non-browser client. The owner accepted the saving, but changed the shape: a demo driver is worth more than an upload tool.

## Decision

- **One command, the demo driver, runs the v1 story end to end:**
  1. `seed`: creates a synthetic tenant, its users, customers and invoices.
  2. `ingest-ledger`: uploads a counterparty ledger.
  3. `run-case`: runs the case to a draft (a BCS).
  4. `show`: prints the artifact and its gate result.
  5. `approve`: approves the artifact as an approver role.

  With no subcommand, it runs all five in order. Uploads of any file kind are one more subcommand, `upload`.
- **Every step except `seed` goes through `/api/v1`,** with the same session and RBAC as the browser (ADR-0038). `seed` calls the seeding command from ADR-0028, because v1 has no API for creating tenants. The driver never reads the database.
- **It works in local-only mode by default** (ADR-0036), so the demo needs no external account.
- **The README demo is two commands:** `docker compose up`, then the driver.
- **B13 keeps** the case view, the approval screen and the review queue. The upload pages are dropped.
- **`09` and `10` record** the driver's place in the build plan and the repository layout (PROJECT_CONTEXT O-26).

## Alternatives rejected

| Alternative | Why rejected |
|---|---|
| Browser upload pages (the original B13) | 0.5 day more for something the demo does not need |
| An upload-only command (Hat B's version of OD-4) | Proves the API, but gives no one-command demo |
| A driver that writes to the database directly | It would prove nothing about the API, and would bypass RBAC |

## Consequences

- Contingency returns to 1.0 day. That is exactly build-if-time item 1 (evidence packets).
- The driver is the first non-browser client of `/api/v1`. It exercises ST-06's API cases and ADR-0038's contract test from the first week.
- The eval harness can reuse the driver to create cases.
