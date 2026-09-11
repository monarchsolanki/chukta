# Project Brief (verbatim)

| | |
|---|---|
| **Purpose** | Keeps the original project brief as given, so every hat and every later session works from the same text. |
| **Intended reader** | Anyone working on Chukta who needs the source requirements. |
| **Status** | Frozen. The text below is not edited. |
| **Received** | 2026-09-11 |

**Corrections live elsewhere.** Checkpoint 1 (2026-09-11) changed or corrected parts of this brief. Those changes are recorded in the ADRs and in `00-PRD.md` Appendix C, not edited into the text below. Examples:
- The Reconciliation Agent row says "three-way matches". That means ledger reconciliation. In AP usage, "three-way match" means the buyer's PO/GRN/invoice check (PRD §12).
- Cases are per customer account, not per overdue invoice (ADR-0009).
- The Orchestrator, Statutory and Analyst roles are deterministic code, not LLM agents (ADR-0007).

The brief below starts with the heading "PROJECT BOOTSTRAP" and ends with "Then stop and wait for my confirmation."

---

# PROJECT BOOTSTRAP: Chukta — Agentic Receivables Recovery for Indian MSMEs

## Your task in this session
Produce the **complete design documentation set** for this project. 
**Do NOT write any application code in this phase.** No components, no routes, 
no schema migrations. Documentation, diagrams, contracts, and specs only.

The only files you may create are under `docs/` and a `README.md`.

---

## 1. PRODUCT CONTEXT

### The problem
Indian MSMEs sell on credit terms and get paid in 70–120 days. The owner or 
accountant chases payments manually over WhatsApp and phone. Invoices stall for 
three specific reasons — and these three, not "forgetting to follow up," are the 
product's actual target:

1. **The document wall.** The buyer's accounts-payable team demands invoice copy, 
   PO reference, signed delivery challan, e-way bill, GRN, and a ledger 
   reconciliation statement. The seller takes days to assemble this, or gives up. 
   The invoice ages another 60 days.

2. **The ledger mismatch.** "Our books show ₹4.2L, yours show ₹4.7L." The buyer 
   sends a ledger statement as a PDF or Excel in a different format every quarter. 
   Reconciling means matching across TDS deductions, credit notes, short-pays, 
   timing differences and missing entries. Nobody does it, so the balance freezes.

3. **The unused statutory lever.** Under Section 43B(h) of the Income-tax Act 
   (carried forward as the equivalent provision under the Income-tax Act 2025 from 
   1 April 2026), a buyer who fails to pay a registered **micro or small** 
   enterprise within 15 days (no written agreement) or up to 45 days (with written 
   agreement, per Section 15 of the MSMED Act 2006) loses the tax deduction for 
   that expense in that year. Section 16 of the MSMED Act separately imposes 
   compound interest with monthly rests at three times the RBI-notified bank rate, 
   and that interest is not deductible for the buyer. Most MSMEs do not know this 
   lever exists or how to compute it per invoice.

### Why AI is load-bearing (not decorative)
Each blocker requires reading messy unstructured documents, reasoning across them, 
deciding the next action, and acting. Remove the LLM layer and the product does not 
work. The design docs must make this case explicitly — if any component could be 
replaced by deterministic code without loss, say so and remove the AI from it.

### Non-goals for v1
- No autonomous sending. Every outbound artifact goes through a human approval gate.
- No legal advice. The system surfaces statutory position and drafts for review by 
  the owner or their CA.
- No live ERP integration. CSV/Excel ingestion is primary; a Tally adapter is 
  specified as an interface with a documented contract but is NOT implemented.
- No real customer data. Development runs entirely on synthetic data.

---

## 2. TARGET ARCHITECTURE (design these, don't build them)

### Agent layer — LangGraph orchestrator + six specialists
| Agent | Responsibility |
|---|---|
| Orchestrator | Owns each overdue invoice as a durable long-lived case. Plans next action, routes to specialists, handles human-approval interrupts, resumes after days of waiting. |
| Evidence Agent | Retrieves and assembles the exact document packet the buyer's AP requested; flags what is missing. |
| Reconciliation Agent | Parses counterparty ledger statements in arbitrary formats; three-way matches against internal ledger and bank credits; emits a Balance Confirmation Statement itemised by cause. |
| Statutory Agent | Verifies Udyam classification, computes per-invoice deadlines and Section 16 compound interest, retrieves the governing provision, drafts a citation-grounded notice. |
| Conversation Agent | Classifies inbound replies (promise-to-pay / dispute / document-request / already-paid-with-UTR / short-pay) including Hinglish and fragments; extracts entities; updates case state. |
| Dispute Agent | Investigates a claim against challan, GRN, quality-complaint thread and prior credit notes; returns a validity assessment with evidence. |
| Analyst Agent | Cash forecast, DSO/CEI trends, and "payroll is on the 7th — who do I chase to get there?" |

### RAG layer
- Four corpora: statutory, contractual, evidence (OCR'd scans), conversational.
- **Hybrid retrieval**: BM25 (Postgres FTS) + dense (pgvector) + reranking. Pure 
  vector search fails on exact tokens like "Section 15", "45 days", "PO-4471".
- **Entity graph** (customer → PO → invoice → challan → payment → dispute → credit 
  note) via Postgres recursive CTEs, for relationship queries vector search cannot 
  answer.
- **Multi-hop retrieval** for dispute investigation (typically 4–5 hops).
- **Tenant isolation enforced at the retrieval API layer**, never in the prompt.
- **Citation enforcement**: any statutory or factual claim in an outbound artifact 
  must carry a retrieved source span, or the artifact is blocked before reaching the 
  approval queue.

### Eval / observability layer
Golden dataset (~200 labelled cases), retrieval metrics (recall@k, MRR, citation 
faithfulness), agent trajectory evals, CI regression suite, Langfuse tracing, 
per-case token cost.

### Model routing
Local SLM (Ollama, qwen3.6:27b) for classification/extraction/routing — target ~80% 
of calls. Frontier model reserved for drafting and multi-hop reasoning. Report cost 
per case.

### Stack (decided — do not re-litigate, but flag any genuine blocker)
- App + console: Next.js 15, TypeScript, Prisma, PostgreSQL
- Vector + graph: pgvector + recursive CTEs in the same Postgres instance
- Agent runtime: Python, FastAPI, LangGraph
- Jobs: Redis + BullMQ (Node side) / Celery (Python side) — pick one and justify
- Evals: Langfuse + pytest golden-set harness in CI
- Infra: Docker multi-stage, AWS, Nginx, GitHub Actions, Prometheus
- Interop: MCP server exposing the receivables tools

---

## 3. HOW YOU WILL WORK — THREE ROLES, SEQUENTIAL, WITH CROSS-REVIEW

You will wear three hats in order. Announce which hat you are wearing before each 
deliverable. Do not blur them — each hat has different incentives and the friction 
between them is the point.

### HAT A — SYSTEMS ARCHITECT
Optimises for correctness, clear boundaries, and long-term extensibility.
Deliverables:
- `docs/00-PRD.md`
- `docs/01-ARCHITECTURE.md`
- `docs/02-DATA-MODEL.md`
- `docs/03-AGENT-DESIGN.md`
- `docs/04-RAG-DESIGN.md`
- `docs/05-EVAL-PLAN.md`
- `docs/adr/ADR-0001-*.md` through `ADR-000N-*.md`

### HAT B — IMPLEMENTATION LEAD
Optimises for shippability inside 28 days by one developer. Hostile to scope.
Deliverables:
- `docs/07-API-CONTRACTS.md`
- `docs/08-SYNTHETIC-DATA-SPEC.md`
- `docs/09-BUILD-PLAN.md`
- `docs/10-REPO-STRUCTURE.md`
- `docs/11-TEST-STRATEGY.md`
- `docs/reviews/IMPL-FEASIBILITY-REVIEW.md` — a blunt review of Hat A's docs. Name 
  every feature that will not fit in 28 days and state what you would cut.

### HAT C — SECURITY & COMPLIANCE ENGINEER
Optimises for what goes wrong. Assumes hostile input and careless users.
Deliverables:
- `docs/06-SECURITY-THREAT-MODEL.md`
- `docs/12-DATA-CLASSIFICATION.md`
- `docs/reviews/SEC-REVIEW-ARCH.md` — findings against Hat A's docs, each with 
  severity (Critical/High/Medium/Low) and a concrete mitigation.

Threat model must cover, at minimum:
- **Prompt injection via ingested content.** The buyer's email replies, ledger 
  statements and attached PDFs flow into agent context. These are attacker-
  controllable. Treat all ingested content as untrusted data, never as instructions. 
  Design the mitigation, don't just name the risk.
- **Cross-tenant retrieval leakage** — enforcement point, test strategy, failure mode.
- **Hallucinated statutory citations** in a legal notice. Highest-severity failure in 
  the system. Design the gate.
- PII handling and redaction before any hosted-model call.
- Auth, RBAC (owner / accountant / collections staff / read-only CA), session, audit.
- Secrets management, webhook signature verification, replay protection, rate limiting.
- India **DPDP Act 2023** obligations relevant to holding third-party business and 
  contact data.
- Approval-gate bypass: can any code path send an outbound message without human sign-off?

### Cross-review sequence
1. Hat A produces its full set → **STOP for my review**
2. Hat C reviews Hat A → **STOP**
3. Hat B reviews Hat A for feasibility → **STOP**
4. Hat A revises against both review docs, logs changes in `docs/adr/` → **STOP**
5. Hat B produces its remaining deliverables → **STOP**
6. Final: `README.md` + `docs/INDEX.md`

---

## 4. HARD RULES

1. **Checkpoint discipline.** Stop at every STOP above and wait for my go-ahead. Do 
   not run ahead.
2. **Preflight before any destructive operation.** Read-only check first, report what 
   exists, ask before overwriting or deleting anything.
3. **No invented statutory content.** Where a legal specific is needed, write the 
   claim and tag it `[VERIFY: <exact provision to confirm>]`. I will verify against 
   primary sources. Never state a section number, rate, or deadline you are not 
   certain of without the tag.
4. **No fabricated metrics anywhere.** The PRD must contain a section splitting:
   - *System metrics* — measurable on synthetic data, defensible now
   - *Business outcome metrics* — require a real pilot, explicitly NOT claimable yet
   Every target number in the docs must be labelled as one or the other.
5. **Writing style.** Plain, direct, human. Short sentences. No marketing register, 
   no "leveraging robust solutions," no em-dash asides, no rule-of-three padding. 
   Write like an engineer briefing another engineer.
6. **Git.** Single `main` branch. Conventional commit messages. No `Co-Authored-By` 
   trailers. One commit per completed deliverable, not one per file.
7. **Every doc opens with**: purpose, intended reader, and status (Draft / In Review / 
   Frozen).
8. **Diagrams as Mermaid** inside the markdown. No external image files.
9. If a requirement in this brief is ambiguous or you believe it is wrong, say so 
   before writing the doc. Do not silently reinterpret it.

---

## 5. START HERE

Begin as **HAT A — SYSTEMS ARCHITECT**.

Before writing anything, output:
- Your understanding of the problem in 5 bullets
- Any ambiguity or disagreement you have with this brief
- Your proposed table of contents for `docs/00-PRD.md`

Then stop and wait for my confirmation.
