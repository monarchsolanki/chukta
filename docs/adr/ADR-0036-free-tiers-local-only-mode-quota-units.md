# ADR-0036: Free tiers in v1, a zero-cost local-only mode, and quota units

| | |
|---|---|
| **Purpose** | Records that every v1 dependency is free, how each one upgrades to a paid option later, how the system runs with no external API at all, and how free-tier rate limits are metered and enforced. |
| **Intended reader** | Anyone configuring models, the gateway, the spend ledger or the eval report, and anyone cloning the repo to run it. |
| **Doc status** | In Review |
| **Decision status** | Accepted, 2026-09-13. Records the owner's constraint C-1. |
| **Resolves** | Constraint C-1 (2026-09-13). The frontier-model half of PROJECT_CONTEXT O-06. |
| **Amends** | ADR-0016 (names the default frontier provider), ADR-0030 (spend ledger fields), PRD NFR-14 and SM-22 |
| **Related** | ADR-0021, ADR-0034, ADR-0037; `01` §8, §10; feasibility review §11 |

## Context

On 2026-09-13 the owner required everything in v1 to run on free tiers, with a documented path to paid options. Three things follow. The design must name a free frontier provider. Anyone cloning the public repo must be able to run the system without any account. And on a free tier, the binding limit is requests per minute and per day, not money, so a spend breaker that counts only rupees would never fire.

## Decision

**1. Every v1 dependency is free, and each has a named upgrade path.**

| Component | v1: free | Upgrade path | What changes on upgrade |
|---|---|---|---|
| Local SLM | Ollama with an open-weights model, on the developer's machine | A GPU host in the production target (`01` §10.3) | Configuration and hardware |
| Frontier model | **Google Gemini API free tier** (via Google AI Studio), a Flash-class model. The model ID is configuration. | The same API's paid tier with non-training terms, or another provider behind the gateway's provider interface | Configuration, plus ADR-0037's gate before any real data |
| Tracing | Langfuse Cloud Hobby tier (build-if-time item 3) | A paid Langfuse plan, or self-hosting (ADR-0030) | Configuration and a processing agreement |
| Runtime | Docker Compose on the developer's machine | The AWS production target (`01` §10.3) | Deployment |
| Postgres with pgvector, Redis, MinIO, nginx, the egress proxy | Open source, in Compose | RDS, S3 and managed equivalents | Configuration |
| CI | GitHub Actions, free on a public repo | Paid minutes, if the repo becomes private | Billing only |
| Payments | Razorpay test mode (build-if-time item 2) | Live keys at the pilot (NG7) | Keys and a merchant account |
| Webhook tunnel (only with Razorpay) | A free tunnel tier, for example a Cloudflare quick tunnel | A named tunnel, or a deployed webhook endpoint | Configuration |
| Inbound mail | None. Fixtures are replayed (DF-08). | A provider chosen under `01` A-Q1 | A receiver and an ADR |
| Statute text and bank rates | Public government sources | None needed | None |

Nothing else in v1 has a cost.

**2. A zero-cost local-only mode.** When no hosted model key is configured, the gateway routes drafting to the local SLM, and the system runs end to end with no external API.
- It is the default for anyone who clones the repo. CI uses stub models, as before.
- Every eval report states which mode produced each figure: **local-only** or **hosted**. SM-21 and SM-22 are reported per mode, alongside ADR-0034's cause note.
- A run never switches modes partway, because mixing modes makes every figure in that run ambiguous.

**3. Quota units, not only money.**
- **Every spend ledger row records:** provider, tier, model, request count, tokens, and rupee cost. The cost is zero on a free tier.
- **The circuit breaker checks two kinds of limit:**
  - the per-case caps from NFR-14
  - each provider's quota windows (requests per minute, requests per day, and token windows where the provider defines them)
- **When a quota response arrives** (for example HTTP 429):
  - The gateway honours any retry-after hint.
  - A short window is waited out.
  - An exhausted daily window moves the case to Waiting, with `wake_at` set to the window's reset, and writes an audit event.
  - It never falls back silently to local-only mode (point 2).
- **Limits come from configuration, never from the documents.** Free-tier limits change without notice, and a stale figure in a frozen document is worse than a pointer. PROJECT_CONTEXT O-23 tracks the current limits and terms.

## Alternatives rejected

| Alternative | Why rejected |
|---|---|
| A paid frontier provider in v1 | The owner's constraint C-1 |
| Local-only permanently | The brief routes drafting to a frontier model. Local-only stays as a mode, not the design. |
| Free models through an aggregator | Which models are free changes without notice, so eval runs would not be reproducible |
| Writing current free-tier limits into the docs | They change. Configuration plus O-23 instead. |
| Silent fallback to local when a quota runs out | Mixed-mode figures, and quality changes nobody chose |

## Consequences

- PRD NFR-14 and SM-22 gain quota units and the provider and tier fields (next revision).
- `01` §8 documents the two modes.
- `12` §9.1 gains ADR-0037's pilot-gate condition.
- The feasibility review §11 costs this change against the committed 22 days.
- O-06 keeps only its local-model half.
