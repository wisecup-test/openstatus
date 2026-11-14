---
version: 2
name: cross-cutting-data-access-layers-standardize-libsql-4c56
description: "@libsql/client Adoption: Data Access Layers Standardize Libsql Client"
metadata:
  actual:
    adr_id: 4c56d4aa-245b-4783-a33e-ffc237b86963
    status: accepted
    candidate_group_id: e0108034-3797-4419-a73d-ebc4c6b1fec3
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/**
---

# @libsql/client Adoption: Data Access Layers Standardize Libsql Client

Scope: applies to `packages/db/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-LIBSQL-001** MUST: Data access layers MUST standardize on @libsql/client for database connectivity, schema migrations, and embedded replica synchronization.

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
Claude Code MUST NOT skip or defer verification. Continuous integration pipelines, pull request architectural code reviews, and automated build pipeline failure mechanisms enforce these requirements.
</enforcement>