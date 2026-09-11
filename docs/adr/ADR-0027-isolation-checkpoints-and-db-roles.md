# ADR-0027: Tenant isolation covers checkpoints, and migrations and runtime use separate database roles

| | |
|---|---|
| **Purpose** | Closes the two gaps in tenant isolation: checkpoints outside RLS, and a single connection that would need both schema ownership and RLS. |
| **Intended reader** | Anyone writing migrations, the data-access layer, the checkpointer setup or ST-03. |
| **Doc status** | In Review |
| **Decision status** | Accepted, 2026-09-12 (step 4) |
| **Resolves** | `06` S-07; SR-04; the owner's finding SEC-01 |
| **Amends** | ADR-0012, ADR-0014 |
| **Related** | `01` §4, §7; `06` §3.2; ADR-0023; PROJECT_CONTEXT O-14 |

## Context

ADR-0012 enforced RLS on the `app` schema. ADR-0014 put LangGraph's checkpoints in `agent_runtime`, outside that enforcement (SR-04). The owner then found SEC-01: `06` §3.2 requires a runtime role that owns nothing, but `01` §4 had the Console running migrations, which need DDL rights. One connection string cannot do both.

## Decision

- **There are two database roles.**
  - The **migration role** owns the `app` and `agent_runtime` schemas. It runs Prisma migrations and LangGraph's checkpointer setup inside a one-shot `migrate` job in Compose. No running service ever uses it.
  - The **runtime role** owns nothing and has no `BYPASSRLS`. It is subject to forced RLS. It has read and write rights on application tables, INSERT only on the audit table (ADR-0023), and read and write rights on the `agent_runtime` tables.
- **Which role each component connects as:**

| Component | Role |
|---|---|
| `migrate` job, one-shot | Migration role |
| Console | Runtime role |
| Agent API | Runtime role |
| Workers and scheduler | Runtime role |

- **A startup check** refuses to run any service connected as a role that owns tables or has `BYPASSRLS` (`06` §3.2). A test asserts that the runtime role cannot run DDL.
- **Checkpoints:**
  - Thread IDs have the form `tenant:case`.
  - A wrapper checks the prefix against the bound tenant on every read and write.
  - RLS policies on the `agent_runtime` tables key on the prefix (S-07).
  - If RLS on LangGraph's own tables fights the library, the prefix wrapper stays, and the RLS part is logged as a finding (feasibility review §9).
- **`02-DATA-MODEL.md` specifies the grants table by table** (O-14).

## Alternatives rejected

| Alternative | Why rejected |
|---|---|
| One role with DDL rights | An owner bypasses RLS, so the isolation guarantee falls |
| A separate database per tenant | ADR-0012 already rejected it for one developer |
| Leave checkpoints unprotected | They hold case state (DC-2, DC-3) |

## Consequences

- ST-03 covers checkpoints and the role checks.
- `01` §4 and §7 are revised: the Console no longer runs migrations.
