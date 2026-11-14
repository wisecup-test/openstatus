---
version: 2
name: cross-cutting-schema-definitions-requiring-region-vali-1a70
description: "Adoption of @openstatus/regions for Centralized Geographic Region Definitions and Validation: Schema Definitions Requiring Region Validation Compose"
metadata:
  actual:
    adr_id: 1a708216-f528-41bc-899c-cfbc617fcb0d
    status: accepted
    candidate_group_id: 12f4f803-fbbd-4c27-ad95-36c34f7a18c8
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/**
---

# Adoption of @openstatus/regions for Centralized Geographic Region Definitions and Validation: Schema Definitions Requiring Region Validation Compose

Scope: applies to `packages/db/src/schema/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-REG-001** SHOULD: Schema definitions requiring region validation SHOULD compose validation primitives directly from the schemas exposed by @openstatus/regions.
- **R-REG-002** MANDATORY: Import regional validation schemas and standard region constants directly from @openstatus/regions when defining plan boundaries or database schemas.
- **R-REG-003** MANDATORY: Ensure default region arrays in plan configurations reference exported constants from the regional module rather than constructing literal arrays.
- **R-REG-004** MANDATORY: Disallow local overrides through static analysis and pull request review checks.

### Verify

```bash
# Discover the repository test runner from the project manifest and execute the test suite covering schema validation.
# Discover the static analysis validation command from the project manifest and run it to verify regional schema imports.
# Discover the module boundary verification script and execute it to ensure no undeclared region identifiers exist.
```

**Accept when:**
- All database schema and plan configuration tests pass with zero validation failures.
- Static analysis confirms all region references resolve directly to the centralized region module without raw string duplicates.
- Dependency checking verifies all schema definitions reference the locked version of the centralized region module.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>