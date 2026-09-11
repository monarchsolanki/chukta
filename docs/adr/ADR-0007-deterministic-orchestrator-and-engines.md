# ADR-0007: The Orchestrator, Statutory and Analyst roles are deterministic code

| | |
|---|---|
| **Purpose** | Records which of the brief's seven agents keep a model and which become plain code, and why. |
| **Intended reader** | Anyone designing or changing agent graphs, routing, statutory computation or analytics. |
| **Doc status** | In Review |
| **Decision status** | Accepted, 2026-09-11 (checkpoint 1) |
| **Related** | PRD §6.2, §6.3, §6.4; ADR-0004, ADR-0008, ADR-0010 |

## Context

The brief describes an LLM orchestrator and six specialists, and asks that any component code can replace without loss should lose its AI. Three of the seven are functions of typed state:
- orchestrator planning and routing
- statutory eligibility, dates and interest
- analytics and chase ranking

## Decision

- **Orchestrator:** a LangGraph `StateGraph` whose conditional edges are plain Python functions over typed case state. Routing is keyed on classified intent and invoice state. Models run only inside nodes.
- **Statutory engine** and **Analyst engine:** deterministic Python modules. They are pure functions of their inputs plus versioned parameters.
- **Models stay where input is unstructured:** Conversation, Evidence (reading requests and scans), Reconciliation (layouts and narrations), Dispute, and prose drafting around slotted facts.
- In these docs, "agent" means a model-driven specialist only.

## Alternatives rejected

| Alternative | Why rejected |
|---|---|
| An LLM planner choosing the next action | Puts nondeterministic control flow around the approval gate. Hard to test, and the audit trail becomes "the model decided". |
| Keep LLM agents, constrained by prompts | A prompt constraint is not a guarantee |
| Drop LangGraph for a hand-written state machine | Possible, but LangGraph provides checkpointing and interrupt/resume, and the stack is decided |

## Consequences

- Every routing decision can be unit-tested with fixed state.
- Trajectory evals test node outputs and the path taken, not planner choices.
- A new action is a code change with a test, not a prompt change.
- Model calls concentrate in fewer places, which helps the SLM share target (SM-21) and cost control (NFR-14).
