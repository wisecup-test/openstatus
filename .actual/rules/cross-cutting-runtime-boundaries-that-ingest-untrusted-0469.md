---
version: 2
name: cross-cutting-runtime-boundaries-that-ingest-untrusted-0469
description: "Zod Schema Definition for Runtime Boundary Validation and Data Contract Enforcement: Runtime Boundaries That Ingest Untrusted External"
metadata:
  actual:
    adr_id: 04698e16-73a7-41a6-925c-1487b4dfafeb
    status: accepted
    candidate_group_id: 3672a080-6f79-4231-a813-e4a5d7bbedcf
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/**
---

# Zod Schema Definition for Runtime Boundary Validation and Data Contract Enforcement: Runtime Boundaries That Ingest Untrusted External

Scope: applies to `apps/web/src/**`.

These rules are ALWAYS ACTIVE for all runtime boundaries that ingest untrusted or external data including incoming HTTP server route handlers, query parameter parsers, request payload consumers, external client responses, environment configuration loaders, and content parsing utilities.

### Rules

- **R-ZOD-001** MUST: All runtime boundaries that ingest untrusted or external data MUST declare authoritative data schemas using Zod validation primitives.

### Verify

```bash
# Execute repository type-checking pipeline discovered from workspace configuration
npm run typecheck

# Execute test runner discovered from project automation scripts
npm test

# Execute linting and static analysis suite
npm run lint
```

**Accept when:**
- All boundary validation schemas successfully compile without type mismatches between inferred schemas and consuming domain functions.
- Test suites verify that malformed payloads are rejected with structured diagnostics and valid payloads are accepted.
- No unvalidated external inputs or unchecked type assertions exist at application entry boundaries.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests containing unvalidated boundary inputs or manual type casts at external boundaries are blocked from merging.
</enforcement>