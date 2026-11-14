---
version: 2
name: cross-cutting-consumers-inspect-repository-lock-artifa-ef51
description: "@libsql/client Adoption: Consumers Inspect Repository Lock Artifact Determine"
metadata:
  actual:
    adr_id: ef51dc03-b453-47fa-8b7c-d39deee34d9a
    status: accepted
    candidate_group_id: e0108034-3797-4419-a73d-ebc4c6b1fec3
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/**
---

# @libsql/client Adoption: Consumers Inspect Repository Lock Artifact Determine

Scope: applies to `packages/db/src/**`.

These rules are ALWAYS ACTIVE for database modules responsible for migrations, data seeding, runtime query execution, and embedded replica synchronization.

### Rules

- **R-LIBSQL-001** MUST: Consumers MUST inspect the repository lock artifact to determine and verify the exact resolved version of @libsql/client prior to calling its APIs.

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
Claude Code MUST NOT skip or defer verification. Continuous integration pipelines verify static types and test suites, and pull requests are validated through architectural code reviews and automated build pipelines.
</enforcement>