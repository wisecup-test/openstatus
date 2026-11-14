---
version: 2
name: cross-cutting-engineers-discover-repository-lock-artif-33c9
description: "Adoption of @openstatus/db/src/schema/constants for Centralized Domain Enumerations: Engineers Discover Repository Lock Artifact Verify"
metadata:
  actual:
    adr_id: 33c9a40a-395a-4fca-b1c4-9fc156b615ab
    status: accepted
    candidate_group_id: 845ebcd2-20cc-4558-8366-edf050da5222
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
      - src/data/**
---

# Adoption of @openstatus/db/src/schema/constants for Centralized Domain Enumerations: Engineers Discover Repository Lock Artifact Verify

Scope: applies to `apps/dashboard/src/components/**`, `apps/dashboard/src/data/**`.

These rules are ALWAYS ACTIVE for validation schemas, data retrieval utilities, user interface components, and application layers integrating with scheduling intervals, regional metadata, or database model definitions referencing shared domain enumerations or database configuration constants.

### Rules

- **R-DB-001** MUST: Engineers MUST discover the repository lock artifact and verify the exact resolved package version of the shared database package before introducing imports or consuming exported constant definitions.

### Verify

```bash
# Discover and run the project static analysis and linting scripts to verify no local duplicates of domain constants are defined in consumer modules.
# Discover and execute the type check and test suites across all workspace packages to verify imported constants satisfy interface contracts.
```

**Accept when:**
- Static type analysis passes across all workspace packages without type errors or resolution failures referencing the schema constants module.
- Automated test suites confirm schema validation correctly accepts canonical enumerations and rejects invalid inputs.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>