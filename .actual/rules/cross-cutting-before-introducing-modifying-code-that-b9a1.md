---
version: 2
name: cross-cutting-before-introducing-modifying-code-that-b9a1
description: "Client-Side Cookie State Access via Custom Hooks: Before Introducing Modifying Code That Interacts"
metadata:
  actual:
    adr_id: b9a1c692-3e4c-477f-9ff9-f462979ed47c
    status: accepted
    candidate_group_id: 6c7ae462-c185-46d7-993b-d6604a9c537b
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/hooks/**
---

# Client-Side Cookie State Access via Custom Hooks: Before Introducing Modifying Code That Interacts

Scope: applies to `packages/ui/src/hooks/**`.

These rules are ALWAYS ACTIVE for client-side user interface components requiring browser cookie synchronization.

### Rules

- **R-COOKIE-001** MUST: Before introducing or modifying code that interacts with versioned core libraries, the consumer MUST inspect the repository lock artifact and verify the exact resolved version against public documentation.
- **R-COOKIE-002** MUST: Encapsulate browser cookie inspection and parsing within a dedicated client-side state hook rather than directly reading and mutating browser cookies inside user interface components.
- **R-COOKIE-003** MUST: Ensure state hooks interacting with browser document objects are guarded against non-browser runtime contexts.
- **R-COOKIE-004** MUST: Validate that cookie names and values are appropriately encoded and decoded when serialized.

### Verify

```bash
# Discover and run the project's static analysis and linting scripts from the repository manifest
# Discover and run the project's unit and hook test suites via the primary repository test runner
```

**Accept when:**
- Client-side cookie synchronization logic encapsulates document object parsing without leaking direct cookie string operations across consumer components.
- Test suites and static analysis pass when executing repository validation routines.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification by automated static code analysis, lint rules, and peer review during code integration checks is mandatory.
</enforcement>