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
- **R-SHARED-002** MANDATORY: Import entity schemas and domain enumeration constants strictly from the designated shared module subpath rather than instantiating duplicate local structures.
- **R-SHARED-003** MANDATORY: Ensure that client components import only static schema definitions and type utilities to prevent bundling runtime database drivers.
- **R-SHARED-004** MANDATORY: The consumer MUST derive all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-SHARED-005** MANDATORY (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, execute in order: 1. Find dependency manifest; 2. Identify build tool; 3. Inspect repository lock or resolution artifact; 4. Look up official documentation/API reference for that exact version; 5. Confirm every API/class/function exists in that version; 6. Re-run steps 3-5 per dependency at point of use.

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