---
version: 2
name: cross-cutting-route-handlers-delegate-domain-data-7dda
description: "Adoption of @openstatus/services Internal Service Library for API Operations: Route Handlers Delegate Domain Data Retrieval"
metadata:
  actual:
    adr_id: 7dda1c4b-cbc8-4c0a-94e7-0ac6a058d506
    status: accepted
    candidate_group_id: 325c1754-ad4f-40ef-aa1d-165da2308b07
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/router/**
---

# Adoption of @openstatus/services Internal Service Library for API Operations: Route Handlers Delegate Domain Data Retrieval

Scope: applies to `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OPENSTATUS-001** MUST: All API route handlers MUST delegate domain data retrieval, state mutations, and business operations to the internal domain service library @openstatus/services rather than executing direct persistence or domain logic within the transport router.

### Verify

```bash
# Discover and execute the test runner script from the configuration manifest
pnpm test
# Discover and execute workspace static analysis and type checking scripts from the configuration manifest
pnpm check
```

**Accept when:**
- All route procedure calls successfully delegate domain logic to @openstatus/services through service context and error adaptation.
- Type checking and static verification across all API modules pass without unresolved imports or structural boundary violations.
- Integration and unit test suites covering API router endpoints pass with zero unhandled exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>