---
version: 2
name: cross-cutting-modules-defining-validating-geographic-r-f033
description: "Adoption of @openstatus/regions for Centralized Geographic Region Definitions and Validation: Modules Defining Validating Geographic Regions Location"
metadata:
  actual:
    adr_id: f033c832-dc81-4a47-b3d8-fae2d548e68e
    status: accepted
    candidate_group_id: 12f4f803-fbbd-4c27-ad95-36c34f7a18c8
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/**
---

# Adoption of @openstatus/regions for Centralized Geographic Region Definitions and Validation: Modules Defining Validating Geographic Regions Location

Scope: applies to `packages/db/src/schema/**`.

These rules are ALWAYS ACTIVE for all modules defining or validating geographic regions, location limits, or regional defaults.

### Rules

- **R-REGION-001** MUST: All modules defining or validating geographic regions, location limits, or regional defaults MUST import standard definitions and schemas exclusively from @openstatus/regions.

### Verify

```bash
# Discover and run the project test suite covering schema validation
# Discover and run static analysis validation to verify regional schema imports
# Discover and run module boundary verification script to ensure no undeclared region identifiers exist
```

**Accept when:**
- All database schema and plan configuration tests pass with zero validation failures.
- Static analysis confirms all region references resolve directly to the centralized region module without raw string duplicates.
- Dependency checking verifies all schema definitions reference the locked version of the centralized region module.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>