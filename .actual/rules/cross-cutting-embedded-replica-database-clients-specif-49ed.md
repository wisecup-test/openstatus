---
version: 2
name: cross-cutting-embedded-replica-database-clients-specif-49ed
description: "@libsql/client Embedded Replica Database Client Adoption: Embedded Replica Database Clients Specify Syncinterval"
metadata:
  actual:
    adr_id: 49eda051-35dc-4742-a9de-d3f70be16d5a
    status: accepted
    candidate_group_id: 841dda31-b2d0-4b1e-abd1-ce373de207ce
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/**
---

# @libsql/client Embedded Replica Database Client Adoption: Embedded Replica Database Clients Specify Syncinterval

Scope: applies to `packages/db/src/**`.

These rules are ALWAYS ACTIVE for database connection initialization, schema migration, database seeding, and data synchronization modules within the database package.

### Rules

- **R-SQL-001** SHOULD: Embedded replica database clients SHOULD specify a syncInterval parameter to automate periodic background replication against the primary database.

### Verify

```bash
# Discover and execute the repository test runner from project manifests for database integration tests
# Discover and execute repository type-checking and linting scripts
```

**Accept when:**
- Database client initialization successfully instantiates local embedded replica instances with remote synchronization parameters.
- Database migration and seeding workflows execute without connection errors against the configured client.
- Repository verification suites and integration checks confirm connectivity and schema synchronization pass without regression.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>