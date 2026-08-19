---
name: to-requirement
description: "Promote an idea or backlog item into a requirement document at docs/requirements/REQ-NNN-slug.md: one file per requirement, platform applicability declared inside, variants for each platform in the same file, references to other requirements."
disable-model-invocation: true
---

# To Requirement

Promote an idea (from `docs/backlog/`, from the current conversation, or from the user directly) into a **requirement document** — the first artifact agents are allowed to process.

The requirement pipeline is: `docs/backlog/` (owner-private, agents never process) → `docs/requirements/` (one file per requirement, this skill's output) → `docs/design/` (decisions, via `/to-design`) → tickets (via `/to-tickets`). If the docs layout is not configured, tell the user to run `/setup-matt-pocock-skills`.

## Process

### 1. Gather the source

Work from the current conversation, or from a backlog document the user names (`docs/backlog/*.md`). A backlog item is raw material: unordered, high-level, possibly contradictory. This skill turns it into a stated requirement — it does not add scope the user didn't ask for.

### 2. Assign the id

Requirements are numbered sequentially across the whole project: `REQ-NNN`. The next id is one more than the highest existing `REQ-` id in `docs/requirements/` **and** `docs/requirements/done/` (scan both — archived requirements still hold their ids). File name: `docs/requirements/REQ-NNN-<kebab-slug>.md`.

### 3. Interview only where the source is silent

You are synthesizing, not interrogating — but requirements have two fields a raw idea never fills in. Ask about each only if you can't infer it:

- **Platform applicability** — which platforms does this requirement apply to? If the wording differs per platform (e.g. a richer mobile variant, a narrower web variant), both variants live in **the same file**, one subsection per platform. Never split a requirement across files by platform.
- **Relationships** — does this requirement build on, extend, or refine an existing one? References are by id: `Builds on: REQ-004`. Cross-reference both directions when you can (edit the referenced file's "Referenced by" line).

Everything else (problem, behavior, constraints) you should be able to draft from the source; show the draft and let the user correct it.

### 4. Write the file

<requirement-template>

# REQ-NNN: <short title>

- Status: open | done (done entries move to `docs/requirements/done/`)
- Platforms: all | web | mobile | server (list each that applies)

## Problem

The problem, from the user's perspective. One or two paragraphs.

## Requirement

What must be true when this is implemented. Testable statements, not implementation
choices — design decisions belong in `docs/design/`, not here.

### <Platform> variant (repeat per platform, only when variants differ)

What differs on this platform. If all platforms share one statement, omit the
subsections entirely.

## References

- Builds on: REQ-NNN (optional)
- Related: REQ-NNN (optional)

## Referenced by

- REQ-NNN (kept up to date by later requirements; may be empty)

</requirement-template>

### 5. Promote or park

- If the source was a backlog item and the user confirms the draft, mark that item **promoted to REQ-NNN** in the backlog file (one-line edit — the backlog is the owner's, so ask before touching it; if they decline, just leave the requirement file).
- Never delete backlog content. The backlog is the owner's private space; this skill only annotates a promotion.

## Rules

- One requirement = one file = one coherent capability. If a promotion naturally splits into two capabilities, write two files and cross-reference them.
- Platform variants share one id and one file. A requirement that applies to all platforms states `Platforms: all` and has no variant subsections.
- Requirements state **what**, never **how**. If you catch yourself writing a module name, an API route, or a schema, stop — that is `/to-design` territory.
- Ids are never reused, even after a requirement is archived to `done/`.
