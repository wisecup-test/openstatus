---
version: 2
name: cross-cutting-presentation-components-define-local-dis-0ea4
description: "Adoption of @openstatus/db/src/schema for Shared Database Schema and Domain Constants: Presentation Components Define Local Display Formatting"
metadata:
  actual:
    adr_id: 0ea40c27-4d09-446a-b5d9-0d8876833033
    status: accepted
    candidate_group_id: e2d352d6-144b-439f-aba4-818e08020193
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/**
---

# Adoption of @openstatus/db/src/schema for Shared Database Schema and Domain Constants: Presentation Components Define Local Display Formatting

Scope: applies to `apps/dashboard/src/**`.

These rules are ALWAYS ACTIVE for any application tier defining or validating database entity structures, client-side form controllers, data table components, and backend route handlers interacting with domain entities.

### Rules

- **R-DB-001** MAY: Presentation components MAY define local display formatting helpers provided they consume types exported from the shared schema module.
- **R-DB-002** MUST: Import entity schemas and domain enumeration constants strictly from the designated shared module subpath rather than instantiating duplicate local structures.
- **R-DB-003** MUST: Ensure that client components import only static schema definitions and type utilities to prevent bundling runtime database drivers.

### Verify

```bash
# Discover and run the project static type analysis script to verify all schema imports and type usages across the workspace.
# Discover and execute the workspace test suite to confirm that validation rules and domain enums match expected behavior across all consuming modules.
```

**Accept when:**
- The repository type checker completes without errors across all modules importing the shared schema definitions.
- All form validation and data presentation tests execute successfully without schema mismatch failures.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration builds fail upon detecting local enum duplication or broken schema type contracts.
</enforcement>