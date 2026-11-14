---
version: 2
name: cross-cutting-cookie-value-parsing-utilize-standardize-a255
description: "Client-Side Cookie State Access via Custom Hooks: Cookie Value Parsing Utilize Standardized String"
metadata:
  actual:
    adr_id: a2556626-bc27-43b1-8fb5-69c88c4d162b
    status: accepted
    candidate_group_id: 6c7ae462-c185-46d7-993b-d6604a9c537b
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/hooks/**
---

# Client-Side Cookie State Access via Custom Hooks: Cookie Value Parsing Utilize Standardized String

Scope: applies to `packages/ui/src/hooks/**`.

These rules are ALWAYS ACTIVE for client-side user interface components requiring browser cookie synchronization.

### Rules

- **R-COOKIE-001** SHOULD: Cookie value parsing SHOULD utilize standardized string matching and encoding routines to prevent parsing errors across varied delimiter formatting.
- **R-COOKIE-002** MANDATORY: State hooks interacting with browser document objects MUST be guarded against non-browser runtime contexts.
- **R-COOKIE-003** MANDATORY: Cookie names and values MUST be appropriately encoded and decoded when serialized.

### Verify

```bash
# Discover and run the project's static analysis and linting scripts from the repository manifest.
# Discover and run the project's unit and hook test suites via the primary repository test runner.
```

**Accept when:**
- Client-side cookie synchronization logic encapsulates document object parsing without leaking direct cookie string operations across consumer components.
- Test suites and static analysis pass when executing repository validation routines.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static code analysis, lint rules, and peer reviews enforce encapsulated client-side cookie hook usage.
</enforcement>