# ADR-0026: MCP is deferred from v1, and its constraints are fixed now

| | |
|---|---|
| **Purpose** | Closes the MCP versus P9 question and records the conditions under which MCP may be built. |
| **Intended reader** | Anyone who later builds the MCP endpoint or its tool contract. |
| **Doc status** | In Review |
| **Decision status** | Accepted, 2026-09-12 (step 4). Amended 2026-09-13 by ADR-0038, which applies the draft rule to every client. |
| **Resolves** | `06` S-01; SR-03; `01` A-Q8; the owner's finding F-01; DF-12; the v1 status of PRD FR-INT-3 |
| **Related** | `01` §2; `06` §3.8 (P-3); ADR-0020 |

## Context

F-01 showed that MCP contradicts P9. An MCP client is usually an LLM app pointed at a hosted model, so data returned by an MCP tool would leave our control. Hat C chose option (a), tightened (S-01). It also found that MCP draft tools could hand an unapproved draft to a client that can send it (SR-03, bypass path P-3). The feasibility review then deferred MCP as DF-12.

## Decision

- **MCP is not built in v1.** Until it is, S-01's option (c) applies: there is no MCP endpoint. DF-12 in `12` §9.2 carries the decision on whether to build it before a pilot.
- **When it is built, MCP must:**
  - be a Console route (ADR-0020)
  - return pseudonymised structured records only
  - never return raw message or document text
  - never return the body of an unapproved draft. Draft tools add the draft to the approval queue and return only an ID and a status.
  - audit every call
  - use short-lived tokens, scoped to one tenant with read and draft rights only, revocable and rate-limited
  - stay disabled for real tenants until the pilot gate passes
- **P9 stands, with no carve-out.** F-01 is closed.

## Alternatives rejected

| Alternative | Why rejected |
|---|---|
| Option (b): a named carve-out in P9 for deliberate user export | Weakens the principle for a feature v1 does not need |
| Build MCP in v1 | 1.5 days, and it proves none of the four claims |

## Consequences

- ST-06's MCP cases become required as soon as MCP is built.
- `07` specifies the tool list with these constraints.
- `01` A-Q8 is closed.
