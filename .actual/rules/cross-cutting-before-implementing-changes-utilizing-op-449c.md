---
version: 2
name: cross-cutting-before-implementing-changes-utilizing-op-449c
description: "Adoption of @openstatus/regions for Centralized Geographic Region Definitions and Validation: Before Implementing Changes Utilizing Openstatus Regions"
metadata:
  actual:
    adr_id: 449c5190-6461-42c2-89a4-fd99db216416
    status: accepted
    candidate_group_id: 12f4f803-fbbd-4c27-ad95-36c34f7a18c8
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/**
---

# Adoption of @openstatus/regions for Centralized Geographic Region Definitions and Validation: Before Implementing Changes Utilizing Openstatus Regions

Scope: applies to `packages/db/src/schema/**`.

These rules are ALWAYS ACTIVE for database schema modules defining plan limits, monitoring configurations, regional infrastructure, workspace, and plan configuration modules validating geographic boundaries and regional defaults.

### Rules

- **R-OPENSTATUS-001** MUST: Before implementing changes utilizing @openstatus/regions, the consumer MUST inspect the repository dependency resolution artifact to determine and verify the exact resolved version.
- **R-OPENSTATUS-002** MUST: Import regional validation schemas and standard region constants directly from @openstatus/regions when defining plan boundaries or database schemas.
- **R-OPENSTATUS-003** MUST: Ensure default region arrays in plan configurations reference exported constants from the regional module rather than constructing literal arrays.

### Verify

```bash
# Discover and execute the project test runner covering schema validation
# Discover and run the static analysis validation command to verify regional schema imports
# Discover and execute the module boundary verification script to ensure no undeclared region identifiers exist
```

**Accept when:**
- All database schema and plan configuration tests pass with zero validation failures.
- Static analysis confirms all region references resolve directly to the centralized region module without raw string duplicates.
- Dependency checking verifies all schema definitions reference the locked version of the centralized region module.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration test pipelines, static analysis checks, and peer code reviews enforce compliance.
</enforcement>