# ADR-0005: The owner supplies verified statute text, and unverified provisions cannot be cited

| | |
|---|---|
| **Purpose** | Records where statutory text comes from and what makes a provision citable. |
| **Intended reader** | Anyone working on the statutory corpus, the loader, the citation gate or statutory tests. |
| **Doc status** | In Review |
| **Decision status** | Accepted, 2026-09-11 (checkpoint 1). Modified: the owner loads text in build Phase 4, using the PRD Appendix B checklist. |
| **Related** | PRD §5.3, FR-STA-6, Appendix A, Appendix B; ADR-0008, ADR-0010 |

## Context

Hard rule 3 of the brief forbids invented statutory content. A hallucinated citation in a legal notice is the highest-severity failure in the system. A model's memory of statute text is not a source, and neither are these docs.

## Decision

- The statutory corpus is loaded **only** from primary sources, by the owner, in build Phase 4. Files use the YAML format in PRD Appendix B.1.
- Each file carries provenance: a source URL on an allowlisted domain, `text_as_of`, verifier and date.
- A provision is `unverified` until the owner marks it `verified`. The gate refuses any citation to a provision that is not verified and in force on the relevant date.
- The corpus starts empty. Before Phase 4, tests use fixture provisions with obviously fake text and `source_type: test_fixture`. The loader refuses fixtures outside the test environment.

## Alternatives rejected

| Alternative | Why rejected |
|---|---|
| Have the model transcribe provisions | Violates rule 3 and cannot be verified |
| Scrape India Code automatically | Risks loading the wrong version, and still needs a human to verify it |
| License a third-party legal database | Cost and licensing for v1, and it still needs verification |

## Consequences

- Statutory drafting cannot be tested end to end until Phase 4 (PRD R-01).
- Test fixtures must never reach a non-test environment. The loader enforces this, and a test covers it.
- Amendments are handled as separate effective-dated versions, not edits.
- Resolving PRD Appendix A happens alongside loading, since the same source pages answer most tags.
