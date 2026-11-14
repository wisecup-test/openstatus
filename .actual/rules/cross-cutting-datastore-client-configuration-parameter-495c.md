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

These rules are ALWAYS ACTIVE for all standalone scripts, operational migration utilities, and background jobs connecting directly to the primary relational datastore.

### Rules

- **R-DS-001** MUST: Datastore client configuration parameters passed to `createClient` MUST be supplied through validated runtime environment configurations rather than hardcoded literals.
- **R-DS-002** MANDATORY: This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.
- **R-DS-003** MANDATORY: LOCK-VERSION GROUNDING — before writing code that uses a versioned library, execute in order:
  1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
  2. Identify the build tool from the manifest.
  3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
  4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
  5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
  6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.

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