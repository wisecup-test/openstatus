---
version: 2
name: cross-cutting-database-access-persistence-operations-w-ad81
description: "drizzle-orm with LibSQL for Database Operations: Database Access Persistence Operations Within Package"
metadata:
  actual:
    adr_id: ad819a94-a674-4db4-8c5f-5e5b3010aaa6
    status: accepted
    candidate_group_id: 546e7b81-0328-4a21-88db-87a6b81e682c
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/**
---

# drizzle-orm with LibSQL for Database Operations: Database Access Persistence Operations Within Package

Scope: applies to `packages/db/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DB-001** MUST: Database access and persistence operations within the database package MUST use drizzle-orm with the appropriate LibSQL driver adapter for query execution and schema management.

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
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated continuous integration pipeline executing type checks and test suites, alongside peer code review.
</enforcement>