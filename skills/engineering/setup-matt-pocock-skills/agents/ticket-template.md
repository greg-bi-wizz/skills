# Ticket template

```markdown
## T-nnn: title
risk: R1 | R2 | R3
source: ADR-NNNN / REQ-NNN / DEF-nnn

### Goal
One sentence. The "why" if not obvious.

### Acceptance criteria
- [ ] testable statement
- [ ] testable statement

### Files
- path/to/change
- path/to/test

### Out of scope
- explicit non-goals (prevents scope creep, gives reviewer its axis-1 ruler)
```

Rules:
- One ticket = one session = one coherent change. Split anything bigger.
- Acceptance criteria must be machine-checkable or human-verifiable, never "works well".
- The `risk:` class decides the process: R1 implementer + owner eyeballs; R2 + reviewer; R3 + reviewer + adversary and qa in parallel. Assigned at creation, by the owner; unsure → R2.
- The `source:` trace names the design doc and requirement the ticket implements. R3 tickets must name them explicitly (adversary's input).
