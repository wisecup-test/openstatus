---
version: 2
name: cross-cutting-database-seeding-synchronization-routine-78ab
description: "drizzle-orm with LibSQL for Database Operations: Database Seeding Synchronization Routines Reuse Common"
metadata:
  actual:
    adr_id: 78abc92d-4830-4641-850c-a5e72b99dab1
    status: accepted
    candidate_group_id: 546e7b81-0328-4a21-88db-87a6b81e682c
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/**
---

# drizzle-orm with LibSQL for Database Operations: Database Seeding Synchronization Routines Reuse Common

Scope: applies to `packages/db/src/**`.

These rules are ALWAYS ACTIVE for all database connection initialization, replication, seeding, and schema validation files matching the configured scope.

### Rules

- **R-DB-001** SHOULD: Database seeding and synchronization routines reuse the common environment credential bindings defined for database connection authentication.

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
Claude Code MUST NOT skip or defer verification. Violation handling includes continuous integration failure blocking code merge upon type mismatch or test failure, and mandatory pull request remediation when unapproved database drivers or unmapped SQL queries are identified.
</enforcement>