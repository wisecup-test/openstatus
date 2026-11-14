---
version: 2
name: cross-cutting-route-handlers-not-bypass-schema-3d2c
description: "Adoption of nuqs for Server-Side URL Search Parameter Parsing and Validation: Route Handlers Not Bypass Schema Validation"
metadata:
  actual:
    adr_id: 3d2c3e5f-7e53-4120-818f-178043cf0746
    status: accepted
    candidate_group_id: b6caa4e1-a734-4f95-91e7-b9071cb6b4c1
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Adoption of nuqs for Server-Side URL Search Parameter Parsing and Validation: Route Handlers Not Bypass Schema Validation

Scope: applies to `apps/status-page/src/app/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NUQS-001** MUST_NOT: Route handlers MUST NOT bypass schema validation by casting raw search query records to expected domain types without executing parser validation.

### Verify

```bash
# Discover and execute the repository static type checker
# Discover and run the project test suite to verify route parameter parsing and validation error handling
```

**Accept when:**
- Static type checking and automated test suites confirm all route search parameters pass through validated cache parser instances.
- No server route handlers access raw search query parameters directly without parsing.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis, linting, and peer code review enforce that route components do not consume direct unparsed query parameters.
</enforcement>