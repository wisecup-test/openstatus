---
version: 2
name: cross-cutting-frontend-forms-data-tables-backend-0a8f
description: "Adoption of @openstatus/db/src/schema for Shared Database Schema and Domain Constants: Frontend Forms Data Tables Backend Handlers"
metadata:
  actual:
    adr_id: 0a8f23d7-5f9d-44de-8d3a-919b70abe366
    status: accepted
    candidate_group_id: e2d352d6-144b-439f-aba4-818e08020193
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/**
---

# Adoption of @openstatus/db/src/schema for Shared Database Schema and Domain Constants: Frontend Forms Data Tables Backend Handlers

Scope: applies to `apps/dashboard/src/**`.

These rules are ALWAYS ACTIVE for any application tier defining or validating database entity structures, client-side form controllers, data table components, and backend route handlers interacting with domain entities.

### Rules

- **R-DB-001** MUST_NOT: Frontend forms, data tables, and backend handlers MUST NOT duplicate or redefine persistent domain enumeration constants locally.

### Verify

```bash
# Discover and run the project static type analysis script to verify all schema imports and type usages across the workspace.
# Discover and execute the workspace test suite to confirm that validation rules and domain enums match expected behavior across all consuming modules.
```

**Accept when:**
- The repository type checker completes without errors across all modules importing the shared schema definitions.
- All form validation and data presentation tests execute successfully without schema mismatch failures.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>