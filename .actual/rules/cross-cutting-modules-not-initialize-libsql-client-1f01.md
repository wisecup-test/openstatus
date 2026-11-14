---
version: 2
name: cross-cutting-modules-not-initialize-libsql-client-1f01
description: "@libsql/client Adoption: Modules Not Initialize Libsql Client Raw"
metadata:
  actual:
    adr_id: 1f017375-0371-4876-8bb1-1f6e09171cb0
    status: accepted
    candidate_group_id: e0108034-3797-4419-a73d-ebc4c6b1fec3
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/**
---

# @libsql/client Adoption: Modules Not Initialize Libsql Client Raw

Scope: applies to `packages/db/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-LIBSQL-001** MUST_NOT: Modules MUST NOT initialize @libsql/client with raw unvalidated credentials and MUST consume validated runtime environment parameters.

### Verify

```bash
# Discover and run the project type-checking script to verify @libsql/client interface compatibility.
# Discover and run the automated test suite to validate client connectivity and migration execution.
# Discover and run the linting suite to ensure database client imports conform to project boundary constraints.
```

**Accept when:**
- Static type checks verify interface compatibility for all client initializations without diagnostics.
- All database connectivity, migration, and replication test suites pass successfully.
- Repository dependency manifests contain no unapproved alternative relational database drivers.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>