---
version: 2
name: cross-cutting-route-components-define-specialized-pars-7812
description: "Adoption of nuqs for Server-Side URL Search Parameter Parsing and Validation: Route Components Define Specialized Parser Cache"
metadata:
  actual:
    adr_id: 78125f07-d76d-47d5-ba12-f3c6ce07d2b7
    status: accepted
    candidate_group_id: b6caa4e1-a734-4f95-91e7-b9071cb6b4c1
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Adoption of nuqs for Server-Side URL Search Parameter Parsing and Validation: Route Components Define Specialized Parser Cache

Scope: applies to `apps/status-page/src/app/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NUQS-001** MAY: Route components MAY define specialized parser cache instances for embedded or sub-domain views where parameter requirements differ from primary public routes.

### Verify

```bash
# Discover and execute the repository static type checker to confirm search parameter parsing adherence
# Discover and run the project test suite to verify route parameter parsing and validation error handling
```

**Accept when:**
- Static type checking and automated test suites confirm all route search parameters pass through validated cache parser instances.
- No server route handlers access raw search query parameters directly without parsing.

<enforcement>
Claude Code MUST NOT skip or defer verification. All route search parameters must pass through validated cache parser instances.
</enforcement>