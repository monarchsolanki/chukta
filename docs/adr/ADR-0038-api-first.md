# ADR-0038: API-first: one deployable serves the UI and a versioned JSON API

| | |
|---|---|
| **Purpose** | Makes the backend a public JSON API from v1, so a mobile client can be built later without a backend rewrite. It fixes the surface, the auth model, and the draft-exposure rule for any client. |
| **Intended reader** | Anyone building Console pages or API routes, writing `07`, or planning a future mobile client. |
| **Doc status** | In Review |
| **Decision status** | Accepted, 2026-09-13. Records the owner's constraint C-2. |
| **Resolves** | Constraint C-2 (2026-09-13) |
| **Amends** | ADR-0020 (D-02's wording), ADR-0028 (the token model), ADR-0025 and ADR-0026 (the draft rule applied to every client); `06` §3.8 P-9 and §6 |
| **Related** | `01` §3, §4; `07`; `11`; feasibility review §11 |

## Context

The owner wants the backend usable by a mobile client later, with no rewrite. Three problems have to be solved now, while they are still cheap:
- A public API could become a second public surface, which is F-02's problem again.
- Retrofitting auth is the expensive rework.
- A mobile client can forward a draft exactly as an MCP client could (SR-03).

No mobile app is built in v1 or in Phase 2. This ADR covers contract and structure only.

## Decision

**1. One deployable, one public surface (D-02 reworded).** The Console deployable serves both the UI and the versioned JSON API at `/api/v1`, from one Next.js process behind the same nginx origin. D-02 now reads:

> "The Console deployable, UI and JSON API together, is the only public application surface. The Agent API stays internal."

A separate API service would be a second public surface to harden, so it is rejected.

**2. The Console consumes its own API.**
- Console pages get data only by calling `/api/v1` through a typed client.
- No module outside the API's data layer imports Prisma. A lint rule enforces this.
- A contract test asserts that every route the Console calls is documented in `07`.

Together these make mobile parity a property of the build, not a promise.

**3. The contract lives in `07`.**
- Resources and verbs, cursor pagination, and one JSON error shape for every failure.
- Versioning by path: only additive changes within `v1`. A breaking change means `v2`.
- An OpenAPI document generated from the route schemas and checked in CI against `07`.

**4. Auth: the token model is specified now, and v1 implements only cookies.**
- **v1:** the cookie session of ADR-0028, with CSRF protection.
- **Specified for later, in `07`:**
  - a short-lived access token
  - a rotating refresh token, bound to a device record
  - revocation per device
  - reuse detection: presenting a refresh token that was already rotated revokes that device
  - refresh tokens stored hashed on the server
- **The API resolves every caller through one principal interface** with two strategies: cookie now, bearer later. Adding the bearer strategy adds a strategy, not a rewrite.
- CSRF checks apply to the cookie strategy only.

**5. SR-03's rule applies to every API client.**
- List and detail endpoints return artifact metadata only.
- **A draft body is returned only by the approval-view endpoint,** for roles allowed to view drafts, with the watermark flag set and an audit event for every view.
- **Share links (`wa.me` or `mailto`) are issued only for approved artifacts.** The endpoint refuses before approval.
- ADR-0025 and ADR-0026 now apply to web, mobile and MCP clients alike.

**6. The P-9 residual grows on mobile, and we say so.** On a phone, watermarking and copy-disable are weaker than in a browser. Screenshots and screen recording are one gesture away, and on-device text recognition defeats copy-disable. A person copying a pending draft (P-9) was accepted as Medium for the web. For a mobile client it is the weakest control on the bypass list. The residual stays accepted, because no technical control can stop a person photographing a screen. What remains:
- every draft view is audited
- the approval hash binding, which guarantees that only the approved artifact gets a share link

`06` §3.8 and §6 are revised to say this.

## Alternatives rejected

| Alternative | Why rejected |
|---|---|
| A separate API service | A second public surface (F-02 again) |
| Server components reading the database directly | API parity would not be enforced, and a mobile client would find gaps |
| Building token auth now | Nothing in v1 uses it, and the cost falls inside the committed days |
| Returning draft bodies from detail endpoints | Any client could forward an unapproved draft |

## Consequences

- `01` §3, §4 and D-02 are reworded (next revision).
- `06` §3.8 and §6 update P-9's residual.
- `07` specifies the API and the token model.
- `11` holds the lint rule and the contract test.
- The feasibility review §11 costs this change.
