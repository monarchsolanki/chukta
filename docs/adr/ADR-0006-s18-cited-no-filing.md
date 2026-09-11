# ADR-0006: Notices cite s.18, with no filing workflow

| | |
|---|---|
| **Purpose** | Records why the formal notice cites the MSEFC reference route and why Chukta builds no filing workflow. |
| **Intended reader** | Anyone working on notice templates or considering MSEFC or Samadhaan features. |
| **Doc status** | In Review |
| **Decision status** | Accepted, 2026-09-11 (checkpoint 1) |
| **Related** | PRD §5.8 (L4), NG6, Appendix B rows 6 and 7; ADR-0005 |

## Context

The brief covers s.15, s.16 and the 43B(h) rule, but not the enforcement route. As we read the Act, a party to a dispute over an amount due may refer it to the Micro and Small Enterprises Facilitation Council [VERIFY V16: MSMED Act 2006 s.18]. The buyer's liability to pay with interest sits in s.17 [VERIFY V15]. A notice that cites s.16 interest but gives no next step is weaker than one that does. Building a filing workflow is legal-process work, and it is out of scope for v1.

## Decision

- s.17 and s.18 are citable provisions in the L4 template only.
- The wording states the supplier's right to refer, as a fact. It carries no threat language and no invented deadline.
- There is no filing workflow and no Samadhaan integration (NG6).
- L4 goes to CA review by default (PRD §7.2).

## Alternatives rejected

| Alternative | Why rejected |
|---|---|
| Omit s.18 | The notice has no next step |
| Build a filing flow to MSEFC or Samadhaan | Legal process that needs legal review. Out of v1 scope. |

## Consequences

- Appendix B loads s.17 and s.18, and the gate treats them like any other provision.
- A filing workflow stays in PROJECT_CONTEXT "After v1".
