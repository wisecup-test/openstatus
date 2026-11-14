---
version: 2
name: cross-cutting-route-handlers-that-encounter-invalid-edf3
description: "Adoption of next/navigation for Route Control in Server Handlers: Route Handlers That Encounter Invalid Parameters"
metadata:
  actual:
    adr_id: edf3bdd6-4b9a-48dc-9c2e-a31d80dda3ca
    status: accepted
    candidate_group_id: 064cba9e-e3a9-42d4-8a30-a0da305cf972
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Adoption of next/navigation for Route Control in Server Handlers: Route Handlers That Encounter Invalid Parameters

Scope: applies to `apps/status-page/src/app/**`.

These rules are ALWAYS ACTIVE for server-side route handlers and endpoint handlers within the application framework that evaluate access preconditions and enforce route-level boundary conditions.

### Rules

- **R-NAV-001** MUST: Route handlers that encounter invalid parameters, unauthorized access states, or missing entities MUST invoke notFound from next/navigation to trigger the standard framework not-found boundary.
- **R-NAV-002** MUST: Invoke framework navigation primitives such as notFound or redirect directly in route handlers prior to executing expensive feed aggregation logic.
- **R-NAV-003** MUST: Verify that internal framework navigation errors are not intercepted or transformed into generic server error responses when wrapping endpoint logic in error-handling blocks.

### Verify

```bash
# Discover and execute the project's static analysis and type checking script
# (Derived from repository configuration)

# Discover and execute the project's automated test suite
# (Derived from repository configuration)
```

**Accept when:**
- Static analysis passes with zero type errors or unresolved module errors for navigation imports across server route handlers.
- Automated test suites confirm that route handlers invoke navigation boundary primitives when preconditions or access parameters are unsatisfied.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated continuous integration checks executing static analysis and test validation suites.
</enforcement>