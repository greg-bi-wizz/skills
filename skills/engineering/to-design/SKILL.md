---
name: to-design
description: "Turn a requirement into design documents: one ADR per decision at docs/design/ADR-NNNN-slug.md, each with a trace to the requirement it decides. No interview, just synthesis of what you've already discussed."
disable-model-invocation: true
---

# To Design

Take a requirement document (`docs/requirements/REQ-NNN-*.md`) and the current conversation, and produce **design documents**: one ADR per decision, each traceable to its requirement.

This is the step between requirements and tickets: requirements say *what* (per file in `docs/requirements/`), design says *how* (per decision in `docs/design/`), tickets say *what to build now* (on the tracker, via `/to-tickets`). If the docs layout is not configured, tell the user to run `/setup-matt-pocock-skills`.

Do NOT interview the user — synthesize what you already know. An exception: the seam check in step 2.

## Process

### 1. Read the requirement

Read the requirement file the user names (or that the conversation is about), plus every requirement its `References` list names. Use the project's domain glossary vocabulary throughout, and respect existing ADRs in `docs/design/` (and `docs/design/done/`) that touch the area — a new ADR may supersede an old one, but must say so explicitly.

### 2. Sketch the seams

Sketch the seams at which the feature will be tested. Prefer existing seams; use the highest seam possible. If new seams are needed, propose them at the highest point you can. The fewer seams across the codebase, the better — the ideal number is one.

**Check with the user that these seams match their expectations.** This is the one point where you pause for input.

### 3. Identify the decisions

A requirement rarely yields one decision. Split the design into independent decisions — each becomes its own ADR. Typical splits: a data model decision, an API contract decision, a client behavior decision, a testing strategy decision. Two decisions that only make sense together are one ADR; two decisions that could each change independently are two.

### 4. Write the ADRs

Ids are sequential across the project: `ADR-NNNN`. The next id is one more than the highest existing `ADR-` id in `docs/design/` **and** `docs/design/done/` (scan both — archived decisions still hold their ids). File name: `docs/design/ADR-NNNN-<kebab-slug>.md`.

One file per decision, using the template below. Every ADR traces to at least one requirement; an implementation detail with no requirement to trace to is a sign it belongs inside another ADR, not in a file of its own.

<adr-template>

# ADR-NNNN: <decision title>

- Status: proposed | accepted (superseded decisions move to `docs/design/done/`)
- Date: YYYY-MM-DD
- Requirements: REQ-NNN (the requirement(s) this decision serves)
- Supersedes: ADR-NNNN (optional)

## Context

What forces this decision: the requirement's wording, the existing code, prior ADRs,
the constraints. Short.

## Decision

The decision, stated in one paragraph, then its consequences:

- What becomes true
- What gets easier
- What gets harder or impossible

Keep implementation-detail prose out of file paths and code snippets where possible —
they go stale fast. Exception: if a prototype produced a snippet that encodes the
decision more precisely than prose (state machine, reducer, schema, type shape), inline
it and note that it came from a prototype. Trim to the decision-rich parts.

## Testing

How this decision will be verified: the seams it is tested at, what makes a good test
here (external behavior, not implementation details), and prior art in the codebase
(similar tests to model on).

</adr-template>

### 5. Link back

For each ADR written, append its id to the requirement file's "Referenced by" section (creating the section if absent):

```
## Referenced by

- ADR-NNNN (one line on what was decided)
```

This is the traceability chain the rest of the pipeline depends on: requirement → design → ticket.

## Rules

- One decision = one file = one id. Never a bundle of unrelated decisions in one ADR.
- Every ADR names at least one `REQ-NNN`. If you can't name one, the decision is either not needed or the requirement is missing — surface that to the user instead of guessing.
- Design docs may contradict a requirement only by superseding it explicitly — and a superseded requirement is a defect in the requirement, which the user must resolve, not you.
- Ids are never reused, even after an ADR is archived to `done/`.
- When a decision is fully implemented and retired, its ADR moves verbatim to `docs/design/done/` — relocation, never rewriting.
