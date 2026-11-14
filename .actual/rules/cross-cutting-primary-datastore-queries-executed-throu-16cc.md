---
version: 2
name: cross-cutting-primary-datastore-queries-executed-throu-16cc
description: "@libsql/client Primary Datastore Client Instantiation: Primary Datastore Queries Executed Through Libsql"
metadata:
  actual:
    adr_id: 16ccfca3-3b5d-47e1-9842-3d1e0b974635
    status: accepted
    candidate_group_id: 57db3caa-0564-41c5-b9bd-ce20d7cb43f6
    evidence:
      source: deep
  scope:
    root: packages/tinybird
    paths:
      - scripts/**
---

# @libsql/client Primary Datastore Client Instantiation: Primary Datastore Queries Executed Through Libsql

Scope: applies to `packages/tinybird/scripts/**`.

These rules are ALWAYS ACTIVE for standalone scripts, operational migration utilities, and background jobs connecting directly to the primary relational datastore.

### Rules

- **R-DS-001** SHOULD: Primary datastore queries executed through `@libsql/client` SHOULD use parameterized query interfaces to prevent injection vulnerabilities and support prepared statement execution.
- **R-DS-002** MANDATORY: The consumer MUST discover tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-DS-003** MANDATORY: Before writing code that uses a versioned library, execute in order: (1) Find the dependency manifest in the repo. (2) Identify the build tool. (3) Inspect the repository lock or resolution artifact to determine the exact resolved version. (4) Look up official docs for that exact version. (5) Confirm every API, class, or function exists in that version. (6) Re-run steps 3-5 per dependency at point of use for version-sensitive behavior.
- **R-DS-004** MANDATORY: Initialize `@libsql/client` instances via `createClient` using validated connection URLs and authentication tokens parsed from runtime configurations.
- **R-DS-005** MANDATORY: Implement defensive error handling around datastore queries to manage transient network disconnects during batch data processing.

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