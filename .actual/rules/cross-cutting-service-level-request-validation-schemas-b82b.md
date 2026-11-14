---
version: 2
name: cross-cutting-service-level-request-validation-schemas-b82b
description: "Adoption of @openstatus/db/src/schema Module for Centralized Service Layer Domain and Plan Schemas: Service Level Request Validation Schemas Compose"
metadata:
  actual:
    adr_id: b82b4bb2-58c1-4dae-91a3-a8504fdc0dc9
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

# Adoption of @openstatus/db/src/schema Module for Centralized Service Layer Domain and Plan Schemas: Service Level Request Validation Schemas Compose

Scope: applies to `packages/services/src/import/**`, `packages/services/src/workspace/**`.

These rules are ALWAYS ACTIVE for service layer modules defining input validation, data structures, or plan limit calculations related to database entities, and internal workspace packages that consume shared data models from the database package.

### Rules

- **R-DB-001** MUST: Service-level request validation schemas MUST compose constraints around entity definitions imported from the shared database schema module.

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