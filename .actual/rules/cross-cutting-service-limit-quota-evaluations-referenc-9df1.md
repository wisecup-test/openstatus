---
version: 2
name: cross-cutting-service-limit-quota-evaluations-referenc-9df1
description: "Adoption of @openstatus/db/src/schema Module for Centralized Service Layer Domain and Plan Schemas: Service Limit Quota Evaluations Reference Plan"
metadata:
  actual:
    adr_id: 9df13e90-5d6c-4032-b4d7-7f84c39c3be7
    status: accepted
    candidate_group_id: 01f978a3-302d-4828-9836-80d54041a4a9
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/import/**
      - src/workspace/**
---

# Adoption of @openstatus/db/src/schema Module for Centralized Service Layer Domain and Plan Schemas: Service Limit Quota Evaluations Reference Plan

Scope: applies to `packages/services/src/import/**`, `packages/services/src/workspace/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OPENSTATUS-001** MUST: Service limit and quota evaluations MUST reference plan thresholds and attributes derived directly from centralized plan schema definitions in @openstatus/db.

### Verify

```bash
# Discover and execute the repository static type checker and linting suite to ensure that all service schema imports resolve to the shared database schema module without unresolved paths.
# Discover and execute the workspace test runner to verify that service-level validation schemas and quota limit checks conform to the shared database and plan schemas.
```

**Accept when:**
- All service modules consuming database entities and plan definitions import them directly from the centralized database schema module.
- The project static analysis and test validation suites pass with zero errors across all workspace packages that consume the shared database schema.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>