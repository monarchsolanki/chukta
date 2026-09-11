# ADR-0014: Agent-runtime tables live in a separate Postgres schema

| | |
|---|---|
| **Purpose** | Records who owns which tables and migrations, so Prisma and LangGraph never fight over the database. |
| **Intended reader** | Anyone working on migrations, the data model, the Python data layer or database operations. |
| **Doc status** | In Review |
| **Decision status** | Accepted, 2026-09-11 (checkpoint 1). Amended at step 4 by ADR-0027 (2026-09-12). |
| **Related** | PRD §5.9; ADR-0012, ADR-0013 |

## Context

Prisma owns the app schema and its migrations. LangGraph's Postgres checkpointer creates its own tables on setup. `prisma migrate dev` treats tables it did not create as drift, and it offers to reset the database. Python also needs to read and write app tables. pgvector columns are not a native Prisma type.

## Decision

- LangGraph checkpoint tables live in a separate Postgres schema, `agent_runtime`. Prisma does not manage it.
- **Prisma is the only migration owner** for app tables in the app schema. That includes the outbox and `wake_at` tables.
- Python reads and writes app tables through SQLAlchemy, mapped to the Prisma-owned schema. It never migrates them.
- pgvector columns are declared `Unsupported("vector(n)")` in Prisma and written only by Python.

## Alternatives rejected

| Alternative | Why rejected |
|---|---|
| Everything in one schema | Prisma's drift detection offers a reset that would wipe LangGraph's tables |
| Alembic owns everything and Prisma only introspects | Two ecosystems fighting, and the Next.js side loses type-safe migrations |
| A separate database for the agent runtime | Adds operations work and loses same-server simplicity, for no isolation we need |

## Consequences

- CI runs `prisma migrate diff` against a migrated database and fails on any drift.
- The LangGraph checkpointer is configured with `search_path` set to `agent_runtime`.
- `02-DATA-MODEL.md` records which schema owns every table.
- Backups and restores cover both schemas.
