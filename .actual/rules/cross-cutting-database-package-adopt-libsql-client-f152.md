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

- **R-LIBSQL-001** MUST: The database package MUST adopt @libsql/client as the standard client library for all database connectivity and embedded replica synchronization operations.
- **R-LIBSQL-002** MANDATORY: The consumer MUST discover tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-LIBSQL-003** MANDATORY (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, execute in order: find dependency manifest, identify build tool, inspect repository lock or resolution artifact for exact resolved version, look up official documentation for that exact version, confirm every API/class/function exists in that version, and re-run steps per dependency at point of use.
- **R-LIBSQL-004** MUST: Encapsulate database client creation in a centralized database package module to prevent divergent replication configurations across features.
- **R-LIBSQL-005** MUST: Ensure environment configuration schemas strictly validate the presence of database remote endpoint URLs and authentication tokens before client instantiation.

### Verify

```bash
# Discover the repository test runner from the project manifests and execute the database integration test suite.
# Discover and execute the repository type-checking and linting scripts to verify database client instantiation conformance.
```

**Accept when:**
- Database client initialization successfully instantiates local embedded replica instances with remote synchronization parameters.
- Database migration and seeding workflows execute without connection errors against the configured client.
- Repository verification suites and integration checks confirm connectivity and schema synchronization pass without regression.

<enforcement>
Claude Code MUST NOT skip or defer verification. Pull requests introducing unapproved database client libraries or direct un-replicated database connections are blocked in automated checks.
</enforcement>