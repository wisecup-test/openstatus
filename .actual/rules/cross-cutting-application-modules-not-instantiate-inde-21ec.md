---
version: 2
name: cross-cutting-application-modules-not-instantiate-inde-21ec
description: "Adoption of @openstatus/db as Centralized Persistence and Schema Module: Application Modules Not Instantiate Independent Database"
metadata:
  actual:
    adr_id: 21ec178d-fc4f-45ea-83c6-9533c95d2bd4
    status: accepted
    candidate_group_id: da7d71f2-7d1f-437e-9659-519d2966bdda
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/**
---

# Adoption of @openstatus/db as Centralized Persistence and Schema Module: Application Modules Not Instantiate Independent Database

Scope: applies to `apps/dashboard/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DB-001** MUST_NOT: Application modules MUST NOT instantiate independent database connection pools or introduce direct database driver dependencies outside of @openstatus/db.

### Verify

```bash
# Discover and execute the repository static analysis and type-checking suites across all workspace packages to verify compliance with @openstatus/db import boundaries.
# Discover and run the project automated integration test suite to validate persistence queries and schema integrity.
# Discover and execute the dependency boundary inspection script to confirm that no unapproved database drivers are imported in application packages.
```

**Accept when:**
- Static analysis confirms that database queries, transactions, and relational schema imports originate strictly from the @openstatus/db package.
- Automated integration test suites pass without database connection pool exhaustion or schema mismatch errors.
- Repository dependency boundary validation verifies no direct database driver dependencies exist in consuming application modules.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>