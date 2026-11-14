---
version: 2
name: cross-cutting-server-route-handlers-not-catch-041a
description: "Adoption of next/navigation for Route Control in Server Handlers: Server Route Handlers Not Catch Suppress"
metadata:
  actual:
    adr_id: 041a6d3c-d2d1-4dc7-b1ee-7392cc100ea8
    status: accepted
    candidate_group_id: 064cba9e-e3a9-42d4-8a30-a0da305cf972
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Adoption of next/navigation for Route Control in Server Handlers: Server Route Handlers Not Catch Suppress

Scope: applies to `apps/status-page/src/app/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NAV-001** MUST_NOT: Server route handlers MUST NOT catch or suppress framework control flow exceptions thrown by next/navigation primitives, allowing the framework runtime to intercept and process the navigation boundary.

### Verify

```bash
# Discover and execute the project's static analysis and type checking script
# (e.g., npm run lint or npx tsc --noEmit)
# Discover and execute the project's automated test suite
# (e.g., npm test)
```

**Accept when:**
- Static analysis passes with zero type errors or unresolved module errors for navigation imports across server route handlers.
- Automated test suites confirm that route handlers invoke navigation boundary primitives when preconditions or access parameters are unsatisfied.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>