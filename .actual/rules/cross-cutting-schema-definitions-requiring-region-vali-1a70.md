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

These rules are ALWAYS ACTIVE for database schema definitions, plan configurations, workspace configurations, and validation layers requiring geographic regions.

### Rules

- **R-REGION-001** SHOULD: Schema definitions requiring region validation SHOULD compose validation primitives directly from the schemas exposed by @openstatus/regions.
- **R-REGION-002** MANDATORY: Import regional validation schemas and standard region constants directly from @openstatus/regions when defining plan boundaries or database schemas.
- **R-REGION-003** MANDATORY: Ensure default region arrays in plan configurations reference exported constants from the regional module rather than constructing literal arrays.
- **R-REGION-004** MANDATORY: Execute the lock-version grounding process before writing code that uses a versioned library: find dependency manifest, identify build tool, inspect repository lock/resolution artifact for exact version, look up official documentation for that exact version, confirm every API/class/function exists in that version, and re-run per dependency at point of use.
- **R-REGION-005** MANDATORY: Disallow local region overrides through static analysis and pull request review checks.

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
Claude Code MUST NOT skip or defer verification. Verification is mandatory via continuous integration test pipelines, static analysis checks detecting raw region string literals, and peer code review.
</enforcement>