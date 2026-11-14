---
version: 2
name: cross-cutting-stateless-serverless-database-connection-89f1
description: "drizzle-orm with LibSQL for Database Operations: Stateless Serverless Database Connections Instantiate Drizzle"
metadata:
  actual:
    adr_id: 89f183e7-a7e3-4ed2-bf26-0eb3adcf93b3
    status: accepted
    candidate_group_id: 546e7b81-0328-4a21-88db-87a6b81e682c
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/**
---

# drizzle-orm with LibSQL for Database Operations: Stateless Serverless Database Connections Instantiate Drizzle

Scope: applies to `packages/db/src/**`.

These rules are ALWAYS ACTIVE for all database connection initialization, query construction, local embedded replication, background synchronization workers, and database seeding/validation utilities.

### Rules

- **R-DB-001** MUST: Stateless or serverless database connections MUST instantiate drizzle-orm through the drizzle-orm/libsql/http driver module.

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
Claude Code MUST NOT skip or defer verification.
</enforcement>