---
version: 2
name: cross-cutting-additions-updates-supported-regional-inf-9ce8
description: "Adoption of @openstatus/regions for Centralized Geographic Region Definitions and Validation: Additions Updates Supported Regional Infrastructure Introduced"
metadata:
  actual:
    adr_id: 9ce8a2f7-3e2e-49c5-86f4-0f8f361628e1
    status: accepted
    candidate_group_id: 12f4f803-fbbd-4c27-ad95-36c34f7a18c8
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/**
---

# Adoption of @openstatus/regions for Centralized Geographic Region Definitions and Validation: Additions Updates Supported Regional Infrastructure Introduced

Scope: applies to `packages/db/src/schema/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-REG-001** MAY: Additions or updates to supported regional infrastructure MAY be introduced only by updating @openstatus/regions rather than local overrides.

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