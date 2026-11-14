---
version: 2
name: cross-cutting-client-side-validation-schemas-presentat-da2f
description: "Adoption of @openstatus/db/src/schema for Shared Database Schema and Domain Constants: Client Side Validation Schemas Presentation Filters"
metadata:
  actual:
    adr_id: da2f824c-1b54-4118-a09e-24758bcbfe5d
    status: accepted
    candidate_group_id: e2d352d6-144b-439f-aba4-818e08020193
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/**
---

# Adoption of @openstatus/db/src/schema for Shared Database Schema and Domain Constants: Client Side Validation Schemas Presentation Filters

Scope: applies to `apps/dashboard/src/**`.

These rules are ALWAYS ACTIVE for any application tier defining or validating database entity structures, client-side form controllers, data table components, and backend route handlers interacting with domain entities.

### Rules

- **R-SHARED-001** SHOULD: Client-side validation schemas and presentation filters SHOULD derive their enumeration definitions and field constraints directly from the shared schema exports.
- **R-SHARED-002** MUST: Import entity schemas and domain enumeration constants strictly from the designated shared module subpath rather than instantiating duplicate local structures.
- **R-SHARED-003** MUST: Ensure that client components import only static schema definitions and type utilities to prevent bundling runtime database drivers.

### Verify

```bash
# Discover and run the project static type analysis script to verify all schema imports and type usages
# Discover and execute the workspace test suite to confirm validation rules and domain enums match expected behavior
```

**Accept when:**
- The repository type checker completes without errors across all modules importing the shared schema definitions.
- All form validation and data presentation tests execute successfully without schema mismatch failures.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration builds fail upon detecting local enum duplication or broken schema type contracts.
</enforcement>