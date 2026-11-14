---
version: 2
name: cross-cutting-route-handlers-decouple-domain-validatio-286d
description: "Adoption of next/navigation for Route Control in Server Handlers: Route Handlers Decouple Domain Validation Authorization"
metadata:
  actual:
    adr_id: 286da724-01ab-46b4-9171-a9f00af64844
    status: accepted
    candidate_group_id: 064cba9e-e3a9-42d4-8a30-a0da305cf972
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Adoption of next/navigation for Route Control in Server Handlers: Route Handlers Decouple Domain Validation Authorization

Scope: applies to `apps/status-page/src/app/**`.

These rules are ALWAYS ACTIVE for server-side route handlers, endpoint handlers, and public feed endpoints.

### Rules

- **R-ROUTE-001** SHOULD: Route handlers SHOULD decouple domain validation and authorization evaluation from navigation termination by validating access preconditions prior to executing feed generation or data serialization.

### Verify

```bash
# Discover and execute the project's static analysis and type checking script
# Discover and execute the project's automated test suite
```

**Accept when:**
- Static analysis passes with zero type errors or unresolved module errors for navigation imports across server route handlers.
- Automated test suites confirm that route handlers invoke navigation boundary primitives when preconditions or access parameters are unsatisfied.

<enforcement>
Verification is mandatory. Claude Code MUST NOT skip or defer verification.
</enforcement>