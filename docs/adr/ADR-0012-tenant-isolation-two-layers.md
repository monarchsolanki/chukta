# ADR-0012: Tenant isolation through the retrieval API plus Postgres RLS, with a bound tenant

| | |
|---|---|
| **Purpose** | Records how one seller's data is kept from every other seller, and why there are two enforcement points. |
| **Intended reader** | Anyone working on data access, retrieval, agent tools, background jobs or the threat model. |
| **Doc status** | In Review |
| **Decision status** | Accepted, 2026-09-11 (checkpoint 1) |
| **Related** | PRD §2, NFR-02, SM-02; ADR-0013, ADR-0014 |

## Context

The brief enforces tenant isolation at the retrieval API layer, never in the prompt. A single enforcement point is a single point of failure. And if a model-callable tool accepts a tenant argument, injected content can ask for another tenant's data.

## Decision

- **Primary:** the retrieval API takes the tenant from the authenticated session or from the case record. It never takes the tenant from the request body.
- **Secondary:** Postgres row-level security on every tenant-scoped table, keyed on a transaction-local setting (`SET LOCAL app.tenant_id`) written by the data-access layer. Tables use `FORCE ROW LEVEL SECURITY`, and the app connects as a role that does not own them.
- **Bound tools:** each tool function gets its tenant bound in a closure when the graph starts. No tool signature has a tenant parameter.
- A CA may belong to several tenants, but a session has exactly one active tenant. Switching tenant starts a new session context.
- Background jobs carry the tenant in the job record and set it the same way.

## Alternatives rejected

| Alternative | Why rejected |
|---|---|
| Retrieval API enforcement only | One bug away from a leak |
| A separate database per tenant | The strongest isolation, but heavy operations for one developer. Revisit at pilot scale. |
| A tenant filter in the prompt | Ruled out by the brief. A prompt is not a control. |

## Consequences

- Prisma must set the tenant inside each transaction, and the connection pool must use transaction-local settings.
- pgvector queries and recursive CTEs run under RLS, so their performance must be measured in `04`.
- The SM-02 suite includes direct SQL run with the wrong tenant setting.
