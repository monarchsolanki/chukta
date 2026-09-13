# ADR-0028: Authentication: the v1 minimum, and the full design at the pilot gate

| | |
|---|---|
| **Purpose** | Records what authentication v1 builds, what waits for the pilot gate, and why. |
| **Intended reader** | Anyone building sign-in, sessions, RBAC or ST-07. |
| **Doc status** | In Review |
| **Decision status** | Accepted, 2026-09-12 (step 4). Amended 2026-09-13 by the owner's decision on DLT-01. Amended 2026-09-13 by ADR-0038, which specifies the token model. |
| **Resolves** | `06` S-02; SR-09; the feasibility review §4; DF-01 |
| **Related** | PRD §7.2; `06` §3.5; `12` §9.1 condition 9; ADR-0030 |

## Context

`06` S-02 specified full authentication. The feasibility review costed it at 4.5 days, against 1.0 for what v1 needs: three seeded users, synthetic data, one machine.

## Decision

- **v1 builds:**
  - passwords hashed with Argon2id
  - server-side sessions, rotated on login and on any role change, with CSRF and origin checks
  - server-side RBAC per PRD §7.2, with active-tenant pinning
  - a CLI that creates and resets the seeded users and writes an audit event. This is v1's account-recovery path (SR-09).
  - nginx login rate limits
  - ST-07's role matrix
- **Deferred to the pilot gate as DF-01, Required** (`12` §9.1, condition 9): TOTP MFA, step-up, recovery codes, and ST-07's MFA and step-up cases.
- **Built so the deferral is cheap to undo:** the user table has nullable TOTP fields from day one, and the step-up action list is a single enumeration in code.
- **No no-op step-up check is built.** A check that always passes reads as protection when it is not.
- If Auth.js's credentials provider requires JWT sessions (to be confirmed in `07`), v1 uses a small hand-written session module instead.
- **Loopback only** (amended 2026-09-13, DLT-01): the v1 Compose file publishes the Console port on `127.0.0.1` only. Nothing in v1 needs remote access, so deferring MFA (DF-01) stops being an accepted residual in v1 and becomes a non-issue. The S-12 tunnel, if built, still routes only webhook paths, and it connects to loopback. The seeding CLI generates random passwords, shows them once and never writes them to the repo.

## Alternatives rejected

| Alternative | Why rejected |
|---|---|
| Full S-02 in v1 | 3.5 more days that protect nothing of value on one machine with synthetic data |
| JWT sessions | Revocation is harder, and S-02 asked for server sessions |

## Consequences

- T-10's residual risk in v1 is higher than `06` assumed. That is acceptable only while the data is synthetic, which ADR-0030's real-data gate enforces.
