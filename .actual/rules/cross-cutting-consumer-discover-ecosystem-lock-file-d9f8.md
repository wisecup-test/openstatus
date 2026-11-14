---
version: 2
name: cross-cutting-consumer-discover-ecosystem-lock-file-d9f8
description: "@libsql/client Primary Datastore Client Instantiation: Consumer Discover Ecosystem Lock File Verify"
metadata:
  actual:
    adr_id: d9f8c484-b710-45c1-b439-1f383a4de3f0
    status: accepted
    candidate_group_id: 57db3caa-0564-41c5-b9bd-ce20d7cb43f6
    evidence:
      source: deep
  scope:
    root: packages/tinybird
    paths:
      - scripts/**
---

# @libsql/client Primary Datastore Client Instantiation: Consumer Discover Ecosystem Lock File Verify

Scope: applies to `packages/tinybird/scripts/**`.

These rules are ALWAYS ACTIVE for standalone scripts, operational migration utilities, and background jobs connecting directly to the primary relational datastore.

### Rules

- **R-LIB-001** MUST: The consumer MUST discover the ecosystem lock file and verify the exact resolved version of `@libsql/client` against official documentation before implementation.
- **R-LIB-002** MUST: Initialize `@libsql/client` instances via `createClient` using validated connection URLs and authentication tokens parsed from runtime configurations.
- **R-LIB-003** MUST: Implement defensive error handling around datastore queries to manage transient network disconnects during batch data processing.
- **R-LIB-004** MUST: Reuse client instances across batch iteration cycles and terminate connections properly upon workflow completion to prevent connection pool exhaustion.

### Verify

```bash
# Discover and run the project static analysis and linting verification scripts.
# Discover and run the project type checking verification scripts.
# Discover and run the project test execution suite covering datastore operations.
```

**Accept when:**
- Static analysis and type checking verification scripts complete with zero errors.
- Primary datastore client connections instantiate successfully using validated configuration parameters.
- All automated test suites covering datastore operations pass without failures.

<enforcement>
Claude Code MUST NOT skip or defer verification. Pull requests introducing unapproved database client drivers or unvalidated connection instantiation patterns will be blocked.
</enforcement>