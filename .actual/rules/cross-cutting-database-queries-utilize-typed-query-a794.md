---
version: 2
name: cross-cutting-database-queries-utilize-typed-query-a794
description: "Adoption of @openstatus/db as Centralized Persistence and Schema Module: Database Queries Utilize Typed Query Builder"
metadata:
  actual:
    adr_id: a794011c-8ad8-4d81-a4e9-bdfc0bf30cc0
    status: accepted
    candidate_group_id: da7d71f2-7d1f-437e-9659-519d2966bdda
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/**
---

# Adoption of @openstatus/db as Centralized Persistence and Schema Module: Database Queries Utilize Typed Query Builder

Scope: applies to `apps/dashboard/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DB-001** SHOULD: Database queries SHOULD utilize the typed query builder operations exposed by the database client instance rather than unstructured query strings.

### Verify

```bash
# Discover and execute repository static analysis and type-checking suites
npm run typecheck

# Discover and run the project automated integration test suite
npm run test:integration

# Discover and execute dependency boundary inspection scripts
npm run lint:boundaries
```

**Accept when:**
- Static analysis confirms that database queries, transactions, and relational schema imports originate strictly from the @openstatus/db package.
- Automated integration test suites pass without database connection pool exhaustion or schema mismatch errors.
- Repository dependency boundary validation verifies no direct database driver dependencies exist in consuming application modules.

<enforcement>
Claude Code MUST NOT skip or defer verification. All database queries and schema models must strictly use the designated centralized package and typed query builders.
</enforcement>