---
version: 2
name: cross-cutting-before-implementing-modifying-code-that-d8a9
description: "@libsql/client Embedded Replica Database Client Adoption: Before Implementing Modifying Code That Depends"
metadata:
  actual:
    adr_id: d8a9533b-820d-4b26-8995-3bd6f068ff19
    status: accepted
    candidate_group_id: 841dda31-b2d0-4b1e-abd1-ce373de207ce
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/**
---

# @libsql/client Embedded Replica Database Client Adoption: Before Implementing Modifying Code That Depends

Scope: applies to `packages/db/src/**`.

These rules are ALWAYS ACTIVE for database connection initialization, schema migration, database seeding, and data synchronization modules within the database package.

### Rules

- **R-LIBSQL-001** MUST: Before implementing or modifying code that depends on @libsql/client, developers MUST discover the dependency manifest, inspect the repository lock artifact, and resolve the exact locked version in accordance with the discovery policy.
- **R-LIBSQL-002** MANDATORY: Execute in order to lock-version ground before writing code that uses a versioned library: (1) Find the dependency manifest in the repo. (2) Identify the build tool from the manifest. (3) Inspect the repository lock or resolution artifact to determine the exact resolved version. (4) Look up official documentation, changelog, or public API reference for that exact version. (5) Confirm every API, class, or function called exists in that exact version's documentation. (6) For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- **R-LIBSQL-003** MANDATORY: Encapsulate database client creation in a centralized database package module to prevent divergent replication configurations across features.
- **R-LIBSQL-004** MANDATORY: Ensure environment configuration schemas strictly validate the presence of database remote endpoint URLs and authentication tokens before client instantiation.

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
Claude Code MUST NOT skip or defer verification. Verified by static analysis rules, continuous integration pipelines, and architecture review gates.
</enforcement>