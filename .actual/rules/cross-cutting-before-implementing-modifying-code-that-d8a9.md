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

- **R-LIB-001** MUST: Before implementing or modifying code that depends on @libsql/client, developers MUST discover the dependency manifest, inspect the repository lock artifact, and resolve the exact locked version in accordance with the discovery policy.
- **R-LIB-002** MANDATORY (DISCOVERY POLICY): The consumer MUST derive all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-LIB-003** MANDATORY (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, execute in order: (1) Find dependency manifest, (2) Identify build tool, (3) Inspect repository lock artifact, (4) Look up official documentation/changelog for exact version, (5) Confirm every API/class/function exists in that version's documentation, (6) Re-run steps 3-5 per dependency at point of use for version-sensitive behavior.
- **R-LIB-004** MUST: Encapsulate database client creation in a centralized database package module to prevent divergent replication configurations across features.
- **R-LIB-005** MUST: Ensure environment configuration schemas strictly validate the presence of database remote endpoint URLs and authentication tokens before client instantiation.

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