---
version: 2
name: cross-cutting-datastore-client-configuration-parameter-495c
description: "@libsql/client Primary Datastore Client Instantiation: Datastore Client Configuration Parameters Passed Createclient"
metadata:
  actual:
    adr_id: 495c0215-7cab-4e42-9c9e-9cf8a2721de8
    status: accepted
    candidate_group_id: 57db3caa-0564-41c5-b9bd-ce20d7cb43f6
    evidence:
      source: deep
  scope:
    root: packages/tinybird
    paths:
      - scripts/**
---

# @libsql/client Primary Datastore Client Instantiation: Datastore Client Configuration Parameters Passed Createclient

Scope: applies to `packages/tinybird/scripts/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-LIB-001** MUST: Datastore client configuration parameters passed to `createClient` MUST be supplied through validated runtime environment configurations rather than hardcoded literals.

### Verify

```bash
# Discover and run the project static analysis, type checking, and test suites
# (Commands depend on project repository manifest discovery)
```

**Accept when:**
- Static analysis and type checking verification scripts complete with zero errors.
- Primary datastore client connections instantiate successfully using validated configuration parameters.
- All automated test suites covering datastore operations pass without failures.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration checks and peer code review.
</enforcement>