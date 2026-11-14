---
version: 2
name: cross-cutting-database-queries-mutations-seed-scripts-89d3
description: "drizzle-orm with LibSQL for Database Operations: Database Queries Mutations Seed Scripts Plan"
metadata:
  actual:
    adr_id: 89d3f3cf-4aa3-4e6b-87f2-5835a51de40f
    status: accepted
    candidate_group_id: 546e7b81-0328-4a21-88db-87a6b81e682c
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/**
---

# drizzle-orm with LibSQL for Database Operations: Database Queries Mutations Seed Scripts Plan

Scope: applies to `packages/db/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DB-001** MUST: All database queries, mutations, seed scripts, and plan validations MUST bind directly to the shared internal schema definition module rather than declaring ad-hoc query structures.

### Verify

```bash
# Discover and run the project test suite using the project build tool discovered from the repository manifest.
# Discover and run the project type checking script to verify schema and query type safety across all database modules.
# Discover and run the project linter and validation checks to ensure compliance with database import and module boundary rules.
```

**Accept when:**
- All database queries, seed operations, and client connections compile with zero type errors against the shared schema.
- Database synchronization clients successfully initialize with configured local storage parameters and remote synchronization endpoints.
- Automated tests covering database interactions and workspace schema validations pass without regression.

<enforcement>
Claude Code MUST NOT skip or defer verification. All database queries, mutations, and seed scripts must be verified via automated continuous integration pipeline type checks and test suites.
</enforcement>