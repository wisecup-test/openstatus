---
version: 2
name: cross-cutting-database-package-adopt-libsql-client-f152
description: "@libsql/client Embedded Replica Database Client Adoption: Database Package Adopt Libsql Client Standard"
metadata:
  actual:
    adr_id: f1521394-9d37-4c11-9ec3-6afaea0c01a0
    status: accepted
    candidate_group_id: 841dda31-b2d0-4b1e-abd1-ce373de207ce
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/**
---

# @libsql/client Embedded Replica Database Client Adoption: Database Package Adopt Libsql Client Standard

Scope: applies to `packages/db/src/**`.

These rules are ALWAYS ACTIVE for all database connection initialization, schema migration, database seeding, and data synchronization modules within the database package.

### Rules

- **R-DB-001** MUST: The database package MUST adopt @libsql/client as the standard client library for all database connectivity and embedded replica synchronization operations.

### Verify

```bash
# Discover and execute the repository test runner from project manifests for database integration tests
# Discover and execute type-checking and linting scripts to verify database client instantiation conformance
```

**Accept when:**
- Database client initialization successfully instantiates local embedded replica instances with remote synchronization parameters.
- Database migration and seeding workflows execute without connection errors against the configured client.
- Repository verification suites and integration checks confirm connectivity and schema synchronization pass without regression.

<enforcement>
Claude Code MUST NOT skip or defer verification. All database persistence modules must utilize @libsql/client with approved client configuration patterns.
</enforcement>