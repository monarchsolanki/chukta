# ADR-0016: The local SLM is justified by privacy, the frontier model sits behind an interface, and cost includes GPU time

| | |
|---|---|
| **Purpose** | Records how model calls are routed between the local SLM and a hosted frontier model, and why. |
| **Intended reader** | Anyone working on model calls, redaction, infrastructure sizing or cost reporting. |
| **Doc status** | In Review |
| **Decision status** | Accepted, 2026-09-11 (checkpoint 1) |
| **Related** | PRD §6.2, NFR-03, NFR-14, SM-05, SM-13, SM-21, SM-22; ADR-0015 |

## Context

The brief routes classification, extraction and routing to a local SLM (Ollama, `qwen3.6:27b`), targeting about 80% of calls. It reserves a frontier model for drafting and multi-hop reasoning, and asks for cost per case. We cannot confirm that model tag (PROJECT_CONTEXT O-06). A model of about 27B needs a 24GB-class GPU at 4-bit. On AWS that is an instance billed all month, so at pilot volume it may cost more per case than a hosted small model.

## Decision

- **Route by task.** The local SLM handles reading tasks on raw ingested content: classification, extraction, layout inference and narrations. The frontier model handles dispute reasoning, residuals the SLM cannot explain, and prose drafting.
- **The justification is privacy, not cost.** Raw counterparty content stays on our infrastructure. Hosted calls receive pseudonymised personal identifiers, but keep amounts and document references, because reasoning needs them.
- Model names are configuration behind a provider interface. The default frontier model is named when O-06 closes.
- Cost per case is always reported two ways: hosted API cost only, and hosted plus amortised GPU time (SM-22).
- The SLM share (SM-21) is a target. Per-task routing follows eval results. For example, SM-13 on the human-written set decides whether Hinglish classification stays local.

## Alternatives rejected

| Alternative | Why rejected |
|---|---|
| Hosted models only | Simpler, and perhaps cheaper at pilot volume, but all raw content leaves our infrastructure. A weaker position under DPDP. |
| Local models only | Frontier-level dispute reasoning is unlikely at about 27B, and drafting quality suffers |
| Route by cost alone | Ignores privacy, the stronger reason to run locally |

## Consequences

- The infrastructure plan must include GPU capacity.
- A pseudonymisation layer runs before every hosted call (NFR-03, SM-05).
- Evals run per model per task.
- The spend circuit breaker caps frontier tokens separately (NFR-14).
