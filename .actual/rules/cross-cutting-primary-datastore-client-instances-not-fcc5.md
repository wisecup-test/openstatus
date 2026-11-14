---
version: 2
name: cross-cutting-primary-datastore-client-instances-not-fcc5
description: "@libsql/client Primary Datastore Client Instantiation: Primary Datastore Client Instances Not Initialized"
metadata:
  actual:
    adr_id: fcc54579-670e-4bb0-bc93-83b464210ee0
    status: accepted
    candidate_group_id: 57db3caa-0564-41c5-b9bd-ce20d7cb43f6
    evidence:
      source: deep
  scope:
    root: packages/tinybird
    paths:
      - scripts/**
---

# @libsql/client Primary Datastore Client Instantiation: Primary Datastore Client Instances Not Initialized

Scope: applies to `packages/tinybird/scripts/**`.

These rules are ALWAYS ACTIVE for standalone scripts, operational migration utilities, and background jobs connecting directly to the primary relational datastore.

### Rules

- **R-DS-001** MUST_NOT: Primary datastore client instances MUST NOT be initialized without explicit authentication tokens when connecting to remote database endpoints.

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
Claude Code MUST NOT skip or defer verification.
</enforcement>