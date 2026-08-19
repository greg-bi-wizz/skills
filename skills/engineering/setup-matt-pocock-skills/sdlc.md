# SDLC Pipeline

How work flows from idea to done in this repo, and how much process each piece of work gets. Every agent skill that reads this file should follow the conventions below.

## The pipeline

```
docs/backlog/          docs/requirements/       docs/design/              tickets
(owner-private)  --->  (one file per       --->  (one ADR per        --->  (on the tracker,
                       requirement,             decision,                 with Design:/Requirement:
                       REQ-NNN)                 ADR-NNNN)                 trace + risk class)
```

- **`docs/backlog/`** — the owner's private space (`my-notes.md`, `new features.md`, anything else). High-level, unordered, unprocessed. **Agents never read or process it.** Promotion to a requirement is the owner's call, executed by `/to-requirement`.
- **`docs/requirements/`** — promoted requirements, one file per requirement (`REQ-NNN-<slug>.md`). Platform applicability is declared inside the file; per-platform variants live in the same file, never split across files. Files reference each other by id. Implemented requirements move (verbatim) to `docs/requirements/done/`.
- **`docs/design/`** — design decisions, one ADR per file (`ADR-NNNN-<slug>.md`), each with a `Requirements:` trace to at least one `REQ-NNN`. Produced by `/to-design`. Superseded or fully-implemented decisions move (verbatim) to `docs/design/done/`.
- **Tickets** — created by `/to-tickets` from design docs. Every ticket carries a `Source:` section (`Design: ADR-NNNN`, `Requirement: REQ-NNN`) and a `risk:` class.

Ids are sequential per layer and never reused, including after archiving to `done/`.

## Risk classes

The `risk:` class on a ticket decides how much process it gets. The owner assigns it at ticket-creation time (`/to-tickets` proposes, the owner confirms). When unsure, R2.

| Class | What | Process |
|---|---|---|
| R1 | UI polish, copy, layout, display-only changes the owner can verify at a glance | implementer + owner eyeballs |
| R2 | new routes/features, schema additions, timing/rounding logic | implementer + reviewer |
| R3 | auth, tenancy, data migration, export/import, secrets, anything where a bug destroys data or trust | implementer + reviewer + adversary and qa in parallel |

## Traceability

The chain `REQ-NNN → ADR-NNNN → ticket` is the backbone. Any artifact may reference backwards (a ticket names its ADR; an ADR names its requirement), and requirements keep a `Referenced by:` section that later artifacts maintain. Nothing in the chain is rewritten after the fact — archiving to `done/` is verbatim relocation, never editing.
