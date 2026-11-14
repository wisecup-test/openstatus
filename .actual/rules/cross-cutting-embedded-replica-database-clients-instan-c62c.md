---
version: 2
name: cross-cutting-embedded-replica-database-clients-instan-c62c
description: "@libsql/client Embedded Replica Database Client Adoption: Embedded Replica Database Clients Instantiated Createclient"
metadata:
  actual:
    adr_id: c62cd570-6cdf-4763-8eca-9d7aae580417
    status: accepted
    candidate_group_id: 841dda31-b2d0-4b1e-abd1-ce373de207ce
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/**
---

# @libsql/client Embedded Replica Database Client Adoption: Embedded Replica Database Clients Instantiated Createclient

Scope: applies to `packages/db/src/**`.

These rules are ALWAYS ACTIVE for database connection initialization, schema migration, database seeding, and data synchronization modules within the database package.

### Rules

- **R-LIB-001** MUST: Embedded replica database clients MUST be instantiated using createClient with explicit configurations for local replica URL, remote syncUrl, and authToken.

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
Claude Code MUST NOT skip or defer verification. Verification is mandatory via static analysis rules, continuous integration pipelines, and architecture review gates.
</enforcement>