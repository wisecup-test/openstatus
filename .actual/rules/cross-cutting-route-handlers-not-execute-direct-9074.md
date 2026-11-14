---
version: 2
name: cross-cutting-route-handlers-not-execute-direct-9074
description: "Adoption of @openstatus/services Internal Service Library for API Operations: Route Handlers Not Execute Direct Database"
metadata:
  actual:
    adr_id: 907400c6-3333-4db0-9689-81f800e9daa3
    status: accepted
    candidate_group_id: 325c1754-ad4f-40ef-aa1d-165da2308b07
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/router/**
---

# Adoption of @openstatus/services Internal Service Library for API Operations: Route Handlers Not Execute Direct Database

Scope: applies to `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DB-001** MUST_NOT: API route handlers MUST_NOT execute direct database queries, write raw persistence statements, or bypass domain validation encapsulated within @openstatus/services.

### Verify

```bash
# Discover and execute the workspace test suite covering API routes and service adapters
# Discover and execute workspace static analysis and type checking
```

**Accept when:**
- All route procedure calls successfully delegate domain logic to @openstatus/services through service context and error adaptation.
- Type checking and static verification across all API modules pass without unresolved imports or structural boundary violations.
- Integration and unit test suites covering API router endpoints pass with zero unhandled exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Compliance is enforced via automated static analysis, type verification, and pull request architectural reviews.
</enforcement>