---
version: 2
name: cross-cutting-components-requiring-browser-cookie-pers-ac6f
description: "Client-Side Cookie State Access via Custom Hooks: Components Requiring Browser Cookie Persistence Access"
metadata:
  actual:
    adr_id: ac6ffe59-a658-44b8-96db-33e896f5bcc5
    status: accepted
    candidate_group_id: 6c7ae462-c185-46d7-993b-d6604a9c537b
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/hooks/**
---

# Client-Side Cookie State Access via Custom Hooks: Components Requiring Browser Cookie Persistence Access

Scope: applies to `packages/ui/src/hooks/**`.

These rules are ALWAYS ACTIVE for all client-side user interface components requiring browser cookie synchronization.

### Rules

- **R-COOKIE-001** MUST: Components requiring browser cookie persistence MUST access cookie values through designated client-side state hooks rather than invoking direct document cookie string operations inline.

### Verify

```bash
# Discover and run the project's static analysis, linting, and unit/hook test suites from the repository manifest.
```

**Accept when:**
- Client-side cookie synchronization logic encapsulates document object parsing without leaking direct cookie string operations across consumer components.
- Test suites and static analysis pass when executing repository validation routines.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis, lint rules, and peer review verify compliance. Violations block integration.
</enforcement>