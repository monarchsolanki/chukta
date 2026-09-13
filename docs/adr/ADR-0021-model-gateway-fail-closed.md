# ADR-0021: One model gateway, fail-closed pseudonymisation, and drafting-only hosted calls in v1

| | |
|---|---|
| **Purpose** | Records how every model call is controlled, what may be sent to a hosted model in v1, and what happens when redaction cannot run. |
| **Intended reader** | Anyone writing model calls, prompts or the pseudonymiser. |
| **Doc status** | In Review |
| **Decision status** | Accepted, 2026-09-12 (step 4). Amended 2026-09-13 by the owner's decision on DLT-02. |
| **Resolves** | `01` D-01, D-04, D-09. `06` S-05. SR-05, SR-16. The feasibility review §8 change to ADR-0016. DF-14. |
| **Amends** | ADR-0016 |
| **Related** | `01` §5.4, §8; `06` §3.4; ADR-0032, ADR-0034 |

## Context

ADR-0016 set routing by task and justified the local model on privacy grounds. Hat C then found two gaps. Pseudonymisation had no stated failure behaviour, and SM-05 caught leaks only after they had left (SR-05). The gateway also re-identified replies before tracing them (SR-16). Separately, the feasibility review cut every hosted call that carries free text out of v1.

## Decision

- **Every model call goes through one gateway library.** CI fails if any other module imports a model SDK (D-01).
- **No raw document image goes to a hosted model** (D-04).
- **Low-confidence output falls back to the review queue, not to the frontier model** (D-09).
- **Pseudonymisation fails closed.** A blocking pre-send scan runs the SM-05 detectors on every hosted payload. If any stage is unavailable, the call is refused (S-05, SR-05). The SM-05 scan of logs stays as a second line.
- **Traces are written from the pseudonymised payload, before re-identification** (SR-16).
- **In v1, the only hosted calls are drafting prompts built from slots.** **The invariant is absolute: a hosted model receives only typed facts and templates, never free text of any origin** (amended 2026-09-13, DLT-02). Approved seller text is rendered into the artifact after generation, as a slot sourced from its approved record, so drafting needs no detector and no name-finding pass. Hosted calls that carry free text wait for DF-14, which requires the full pipeline first. Those are the frontier explanation of residuals and dispute reasoning. Phase 2 brings dispute reasoning, and with it DF-14's pipeline (ADR-0032).

## Alternatives rejected

| Alternative | Why rejected |
|---|---|
| A model client per agent | Every agent would reimplement the controls, and one would miss something |
| Leak detection after the fact only | By then the data has already left |
| Building the name-finding pass in v1 | Nothing in v1 would use it |

## Consequences

- ST-05 is small in v1, because it works at slot level.
- ADR-0016's routing table stands, but its free-text rows stay inactive until DF-14 lands.
- In v1, SM-21 (local SLM share) is high for a structural reason, and it must be reported that way (ADR-0034).
- `01` §5.4 and §8 are revised to match.
