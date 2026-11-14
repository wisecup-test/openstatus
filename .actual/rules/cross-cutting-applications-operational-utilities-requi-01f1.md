---
version: 2
name: cross-cutting-applications-operational-utilities-requi-01f1
description: "@libsql/client Primary Datastore Client Instantiation: Applications Operational Utilities Requiring Direct Access"
metadata:
  actual:
    adr_id: 01f18d75-3a52-4005-b42e-40932bb24da2
    status: accepted
    candidate_group_id: 57db3caa-0564-41c5-b9bd-ce20d7cb43f6
    evidence:
      source: deep
  scope:
    root: packages/tinybird
    paths:
      - scripts/**
---

# @libsql/client Primary Datastore Client Instantiation: Applications Operational Utilities Requiring Direct Access

Scope: applies to `packages/tinybird/scripts/**`.

These rules are ALWAYS ACTIVE for standalone scripts, operational migration utilities, and background jobs connecting directly to the primary relational datastore.

### Rules

- **R-CLIENT-001** MUST: Applications and operational utilities requiring direct access to the primary datastore MUST initialize database connections using `@libsql/client` via `createClient`.

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
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks execute static analysis, dependency validation, and peer code review verifying that primary datastore connections use the approved client initialization pattern.
</enforcement>