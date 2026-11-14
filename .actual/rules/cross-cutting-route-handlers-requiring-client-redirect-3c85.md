---
version: 2
name: cross-cutting-route-handlers-requiring-client-redirect-3c85
description: "Adoption of next/navigation for Route Control in Server Handlers: Route Handlers Requiring Client Redirection Invoke"
metadata:
  actual:
    adr_id: 3c85e314-54ef-4832-8690-78fca1e20927
    status: accepted
    candidate_group_id: 064cba9e-e3a9-42d4-8a30-a0da305cf972
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Adoption of next/navigation for Route Control in Server Handlers: Route Handlers Requiring Client Redirection Invoke

Scope: applies to `apps/status-page/src/app/**`.

These rules are ALWAYS ACTIVE for server-side route handlers and endpoint handlers within the application framework, as well as public feed endpoints that evaluate access preconditions and enforce route-level boundary conditions.

### Rules

- **R-NAV-001** MUST: Route handlers requiring client redirection MUST invoke redirect from next/navigation with appropriate navigation semantics rather than returning manual redirect status headers.

### Verify

```bash
# Discover and execute the project's static analysis and type checking script to verify correct import paths and signature compliance for framework navigation primitives.
# Discover and execute the project's automated test suite to ensure server route handlers return expected framework boundary responses upon invalid parameters.
```

**Accept when:**
- Static analysis passes with zero type errors or unresolved module errors for navigation imports across server route handlers.
- Automated test suites confirm that route handlers invoke navigation boundary primitives when preconditions or access parameters are unsatisfied.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification via automated continuous integration checks executing static analysis and test validation suites is mandatory.
</enforcement>