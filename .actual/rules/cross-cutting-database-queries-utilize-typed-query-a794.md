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

These rules are ALWAYS ACTIVE for all application route handlers, authentication integrations, proxies, and operational scripts requiring relational persistence or schema definitions.

### Rules

- **R-DB-001** SHOULD: Database queries SHOULD utilize the typed query builder operations exposed by the database client instance rather than unstructured query strings.

### Verify

```bash
# Discover and execute the repository static analysis and type-checking suites across all workspace packages
npx tsc --noEmit

# Run project automated integration test suites
npm test

# Verify dependency boundaries
npm run lint:boundaries
```

**Accept when:**
- Static analysis confirms that database queries, transactions, and relational schema imports originate strictly from the @openstatus/db package.
- Automated integration test suites pass without database connection pool exhaustion or schema mismatch errors.
- Repository dependency boundary validation verifies no direct database driver dependencies exist in consuming application modules.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated continuous integration checks, static analysis, and peer architectural code reviews.
</enforcement>