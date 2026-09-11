# ADR-0008: The statutory corpus is a verified table, not RAG

| | |
|---|---|
| **Purpose** | Records why statutory text is looked up by ID instead of retrieved by search. |
| **Intended reader** | Anyone working on retrieval, the statutory engine or citations. |
| **Doc status** | In Review |
| **Decision status** | Accepted, 2026-09-11 (checkpoint 1) |
| **Related** | PRD §6.3, Appendix B; ADR-0005, ADR-0010 |

## Context

The brief puts statutory text in one of four RAG corpora with hybrid retrieval. The statutory set is about fifteen provisions (PRD Appendix B). A notice's citations follow from the rule the engine applied, not from a search. Exact tokens like "Section 15" are what dense retrieval handles worst.

## Decision

- Provisions are rows in a versioned table, keyed by `provision_id` and effective dates.
- The engine emits the `provision_id`s it applied. The template renders citations from those IDs, using verified spans.
- Statutory text is never embedded and never retrieved.
- The evidence, contractual and conversational corpora keep hybrid retrieval, reranking, the entity graph and multi-hop retrieval.

## Alternatives rejected

| Alternative | Why rejected |
|---|---|
| Hybrid RAG over statutes, as briefed | Adds a wrong-section failure to the highest-severity path, with nothing to gain at about fifteen rows |
| RAG plus a verifier on the result | More moving parts for no benefit over a direct lookup |

## Consequences

- Citation correctness reduces to one question: did the engine apply the right rule? That is testable (SM-07, SM-08).
- `04-RAG-DESIGN.md` covers three corpora, not four.
- Answering free-form legal questions would need its own ADR, and it borders on legal advice (NG2).
