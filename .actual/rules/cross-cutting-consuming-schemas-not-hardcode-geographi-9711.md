---
version: 2
name: cross-cutting-consuming-schemas-not-hardcode-geographi-9711
description: "Adoption of @openstatus/regions for Centralized Geographic Region Definitions and Validation: Consuming Schemas Not Hardcode Geographic Region"
metadata:
  actual:
    adr_id: 9711623f-7610-4a4b-bcac-41075e3c9216
    status: accepted
    candidate_group_id: 12f4f803-fbbd-4c27-ad95-36c34f7a18c8
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/**
---

# Adoption of @openstatus/regions for Centralized Geographic Region Definitions and Validation: Consuming Schemas Not Hardcode Geographic Region

Scope: applies to `packages/db/src/schema/**`.

These rules are ALWAYS ACTIVE for database schema modules defining plan limits, monitoring configurations, or regional infrastructure, and workspace and plan configuration modules validating geographic boundaries and regional defaults.

### Rules

- **R-REGION-001** MUST_NOT: Consuming schemas MUST NOT hardcode geographic region identifiers, regional lists, or location strings independently of @openstatus/regions.

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