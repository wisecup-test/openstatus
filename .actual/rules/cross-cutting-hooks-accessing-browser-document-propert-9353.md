---
version: 2
name: cross-cutting-hooks-accessing-browser-document-propert-9353
description: "Client-Side Cookie State Access via Custom Hooks: Hooks Accessing Browser Document Properties Designated"
metadata:
  actual:
    adr_id: 9353748a-33d0-4480-85e4-cc1669af67b8
    status: accepted
    candidate_group_id: 6c7ae462-c185-46d7-993b-d6604a9c537b
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/hooks/**
---

# Client-Side Cookie State Access via Custom Hooks: Hooks Accessing Browser Document Properties Designated

Scope: applies to `packages/ui/src/hooks/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-COOKIE-001** MUST: Hooks accessing browser document properties MUST be designated for client-side execution boundaries to prevent execution errors in non-browser runtime environments.

### Verify

```bash
# Discover and run the project's static analysis, linting, and test suites from the repository manifest.
```

**Accept when:**
- Client-side cookie synchronization logic encapsulates document object parsing without leaking direct cookie string operations across consumer components.
- Test suites and static analysis pass when executing repository validation routines.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated static code analysis, lint rules, and peer review during code integration checks.
</enforcement>