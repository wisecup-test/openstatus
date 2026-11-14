---
version: 2
name: cross-cutting-developers-discover-dependency-lock-arti-ebe7
description: "Internal env Module Adoption for Centralized Environment Configuration: Developers Discover Dependency Lock Artifact Repository"
metadata:
  actual:
    adr_id: ebe7556d-345f-4b6d-875c-02a09bb14d0f
    status: accepted
    candidate_group_id: dddf54ae-3635-4cb8-b55c-8fdd1b55302a
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/lib/**
      - src/router/**
---

# Internal env Module Adoption for Centralized Environment Configuration: Developers Discover Dependency Lock Artifact Repository

Scope: applies to `packages/api/src/lib/**`, `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENV-001** MUST: Discover the dependency lock artifact in the repository and verify the exact resolved versions of all internal and external module dependencies before introducing or consuming environment configuration APIs.

### Verify

```bash
# Discover the project test runner and execute the test suite covering API routers and client libraries
# Discover the static analysis and linting verification scripts in the project manifest and execute them
```

**Accept when:**
- All API routers and integration client modules import configuration properties exclusively from the central env module.
- Test execution passes without errors resulting from missing or unvalidated environment credentials.
- Static verification confirms absence of direct ambient process environment access outside permitted bootstrap exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>