---
version: 2
name: cross-cutting-database-entity-definitions-schema-const-c7c7
description: "Adoption of @openstatus/db/src/schema for Shared Database Schema and Domain Constants: Database Entity Definitions Schema Constraints Persistent"
metadata:
  actual:
    adr_id: c7c74d66-386d-4a89-a526-489cdb14ad32
    status: accepted
    candidate_group_id: e2d352d6-144b-439f-aba4-818e08020193
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/**
---

# Adoption of @openstatus/db/src/schema for Shared Database Schema and Domain Constants: Database Entity Definitions Schema Constraints Persistent

Scope: applies to `apps/dashboard/src/**`.

These rules are ALWAYS ACTIVE for any application tier defining or validating database entity structures, client-side form controllers, data table components, and backend route handlers interacting with domain entities.

### Rules

- **R-DB-001** MUST: All database entity definitions, schema constraints, and persistent domain enumeration constants MUST be sourced directly from the shared @openstatus/db/src/schema module.
- **R-DB-002** MUST: Import entity schemas and domain enumeration constants strictly from the designated shared module subpath rather than instantiating duplicate local structures.
- **R-DB-003** MUST: Ensure that client components import only static schema definitions and type utilities to prevent bundling runtime database drivers.

### Verify

```bash
# Discover and run the project static type analysis script to verify all schema imports and type usages across the workspace
npx tsc --noEmit

# Discover and execute the workspace test suite to confirm validation rules and domain enums match expected behavior
npm test
```

**Accept when:**
- The repository type checker completes without errors across all modules importing the shared schema definitions.
- All form validation and data presentation tests execute successfully without schema mismatch failures.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration builds and peer reviews enforce this policy; local enum duplication or broken schema type contracts will fail the build.
</enforcement>