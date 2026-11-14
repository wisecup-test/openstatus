---
version: 2
name: cross-cutting-server-route-handlers-requiring-navigati-e9c2
description: "Adoption of next/navigation for Route Control in Server Handlers: Server Route Handlers Requiring Navigation Control"
metadata:
  actual:
    adr_id: e9c2bd55-5857-4de0-b24c-29aa00558dfc
    status: accepted
    candidate_group_id: 064cba9e-e3a9-42d4-8a30-a0da305cf972
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Adoption of next/navigation for Route Control in Server Handlers: Server Route Handlers Requiring Navigation Control

Scope: applies to `apps/status-page/src/app/**`.

These rules are ALWAYS ACTIVE for server-side route handlers and endpoint handlers within the application framework, as well as public feed endpoints that evaluate access preconditions and enforce route-level boundary conditions.

### Rules

- **R-NAV-001** MUST: Server route handlers requiring navigation control flow or standard termination responses MUST import and invoke control primitives from the next/navigation module rather than manually constructing custom HTTP redirection or status error responses.

### Verify

```bash
# Discover and execute the project's static analysis and type checking script
# Discover and execute the project's automated test suite
```

**Accept when:**
- Static analysis passes with zero type errors or unresolved module errors for navigation imports across server route handlers.
- Automated test suites confirm that route handlers invoke navigation boundary primitives when preconditions or access parameters are unsatisfied.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated continuous integration checks executing static analysis and test validation suites.
</enforcement>