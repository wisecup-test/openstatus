---
version: 2
name: cross-cutting-relational-entity-models-schema-validati-f733
description: "Adoption of @openstatus/db as Centralized Persistence and Schema Module: Relational Entity Models Schema Validation Objects"
metadata:
  actual:
    adr_id: f7339f0f-f219-4c06-b9c9-44be137c6aee
    status: accepted
    candidate_group_id: da7d71f2-7d1f-437e-9659-519d2966bdda
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/**
---

# Adoption of @openstatus/db as Centralized Persistence and Schema Module: Relational Entity Models Schema Validation Objects

Scope: applies to `apps/dashboard/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OPN-001** MUST: Relational entity models and schema validation objects MUST be imported directly from the authorized schema subpath of @openstatus/db.

### Verify

```bash
# Discover and execute the repository static analysis and type-checking suites across all workspace packages
# Discover and run the project automated integration test suite to validate persistence queries and schema integrity
# Discover and execute the dependency boundary inspection script to confirm no unapproved database drivers are imported
```

**Accept when:**
- Static analysis confirms that database queries, transactions, and relational schema imports originate strictly from the @openstatus/db package.
- Automated integration test suites pass without database connection pool exhaustion or schema mismatch errors.
- Repository dependency boundary validation verifies no direct database driver dependencies exist in consuming application modules.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>