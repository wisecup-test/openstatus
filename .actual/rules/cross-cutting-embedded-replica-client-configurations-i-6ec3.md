---
version: 2
name: cross-cutting-embedded-replica-client-configurations-i-6ec3
description: "@libsql/client Adoption: Embedded Replica Client Configurations Initialize Libsql"
metadata:
  actual:
    adr_id: 6ec37df3-1e61-471f-9c4a-d81a50c034ca
    status: accepted
    candidate_group_id: e0108034-3797-4419-a73d-ebc4c6b1fec3
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/**
---

# @libsql/client Adoption: Embedded Replica Client Configurations Initialize Libsql

Scope: applies to `packages/db/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-LIB-001** MUST: Embedded replica client configurations MUST initialize @libsql/client via createClient specifying local database storage URI, remote synchronization URL, authentication token, and synchronization interval parameters.

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