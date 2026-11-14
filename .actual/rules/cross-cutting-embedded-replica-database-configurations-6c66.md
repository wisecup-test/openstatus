---
version: 2
name: cross-cutting-embedded-replica-database-configurations-6c66
description: "drizzle-orm with LibSQL for Database Operations: Embedded Replica Database Configurations Instantiate Createclient"
metadata:
  actual:
    adr_id: 6c669471-7de8-4e0e-82b8-851ff4bd1565
    status: accepted
    candidate_group_id: 546e7b81-0328-4a21-88db-87a6b81e682c
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/**
---

# drizzle-orm with LibSQL for Database Operations: Embedded Replica Database Configurations Instantiate Createclient

Scope: applies to `packages/db/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DB-001** MUST: Embedded replica database configurations MUST instantiate createClient from @libsql/client with designated local storage, synchronization URL, authentication credentials, and synchronization interval parameters before binding to drizzle-orm/libsql.

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