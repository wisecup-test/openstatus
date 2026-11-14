---
version: 2
name: cross-cutting-search-parameter-parser-schemas-centrali-ce25
description: "Adoption of nuqs for Server-Side URL Search Parameter Parsing and Validation: Search Parameter Parser Schemas Centralized Dedicated"
metadata:
  actual:
    adr_id: ce258eae-e2bb-48be-a7e2-c31de7f14e14
    status: accepted
    candidate_group_id: b6caa4e1-a734-4f95-91e7-b9071cb6b4c1
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Adoption of nuqs for Server-Side URL Search Parameter Parsing and Validation: Search Parameter Parser Schemas Centralized Dedicated

Scope: applies to `apps/status-page/src/app/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NQS-001** SHOULD: Search parameter parser schemas SHOULD be centralized in dedicated parameter modules to ensure schema parity between server-side validation and client-side URL state management.

### Verify

```bash
# Discover and execute the repository static type checker to confirm that all search parameter parsing calls adhere to defined schema types.
# Discover and run the project test suite to verify route parameter parsing and validation error handling.
```

**Accept when:**
- Static type checking and automated test suites confirm all route search parameters pass through validated cache parser instances.
- No server route handlers access raw search query parameters directly without parsing.

<enforcement>
Claude Code MUST NOT skip or defer verification. All route search parameters must pass through validated cache parser instances.
</enforcement>