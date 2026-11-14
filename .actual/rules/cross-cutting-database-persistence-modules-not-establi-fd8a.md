---
version: 2
name: cross-cutting-database-persistence-modules-not-establi-fd8a
description: "@libsql/client Embedded Replica Database Client Adoption: Database Persistence Modules Not Establish Unmanaged"
metadata:
  actual:
    adr_id: fd8ab1c4-93ea-4345-8df8-cdf968e02b6a
    status: accepted
    candidate_group_id: 841dda31-b2d0-4b1e-abd1-ce373de207ce
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/**
---

# @libsql/client Embedded Replica Database Client Adoption: Database Persistence Modules Not Establish Unmanaged

Scope: applies to `packages/db/src/**`.

These rules are ALWAYS ACTIVE for all database connection initialization, schema migration, database seeding, and data synchronization modules within the database package.

### Rules

- **R-DB-001** MUST_NOT: Database persistence modules MUST NOT establish unmanaged ad-hoc connections that bypass the centralized client configuration and replica synchronization lifecycle.

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
Claude Code MUST NOT skip or defer verification. Verification by static analysis rules, CI pipelines, and architecture review gates is mandatory.
</enforcement>