# ADR-0020: v1 runtime, network boundary and region

| | |
|---|---|
| **Purpose** | Records where v1 runs, how its network boundary is enforced, and the region of the production target. |
| **Intended reader** | Anyone building the Compose stack, CI or the production target. |
| **Doc status** | In Review |
| **Decision status** | Accepted, 2026-09-12 (step 4). Amended 2026-09-13 by ADR-0038, which rewords D-02. |
| **Resolves** | `01` D-02, D-03, D-06. `06` S-03, S-12, S-14. SR-13, SR-17, SR-18. `01` A-Q2, A-Q3, A-Q7. |
| **Related** | `01` §3, §10; `06` §4; `reviews/IMPL-FEASIBILITY-REVIEW.md` B1; ADR-0025 |

## Context

These decisions were first made in `01` §15, and revised after the owner's findings F-02 and F-03. `06` answered A-Q3 and A-Q7, and the feasibility review confirmed the Compose runtime (A-Q2). Until now they have lived only in tables inside documents, so this ADR records them durably.

## Decision

- **v1 runs with `docker compose up` on one developer machine.** The AWS topology in `01` §10.3 is a specified target, and v1 does not build it (A-Q2).
- **The Console is the only public application surface** (D-02). It receives webhooks, and it serves the MCP endpoint as a route if MCP is built (ADR-0026). The Agent API is internal only.
- **Only workers have internet egress,** through a proxy that allows only listed domains (D-03, S-03). v1 enforces this with Compose networks marked `internal: true`, and CI tests it on every push. The production target adds host `DOCKER-USER` firewall rules. AWS Network Firewall is not used (SK-04).
- **nginx sits on a routable network only to publish its port.** CI asserts that it cannot reach the data network (SR-17).
- **The dev tunnel for live payment webhooks** routes only webhook paths, through an nginx location allowlist, and runs only for that test (S-12, SR-13). It exists only if the Razorpay loop is built (ADR-0025).
- **Images are built natively** on the machine that runs them. The production target pins `linux/amd64` (D-06).
- **Production target region: ap-south-1** (S-14, A-Q7). Every host in the target syncs time over NTP and keeps logs as `12` §3 requires [VERIFY V39: CERT-In Directions of 28 April 2022] (SR-18).

## Alternatives rejected

| Alternative | Why rejected |
|---|---|
| A funded AWS deployment in v1 | The GPU host is unaffordable (F-03) |
| AWS Network Firewall | Adds cost without adding a property the proxy lacks |
| A separate MCP container | A second public surface (the D-02 revision) |

## Consequences

- ST-09 runs on every push, from day one of the build.
- `10-REPO-STRUCTURE.md` carries the Compose network layout.
- The tunnel appears in `09-BUILD-PLAN.md` only alongside the Razorpay item.
