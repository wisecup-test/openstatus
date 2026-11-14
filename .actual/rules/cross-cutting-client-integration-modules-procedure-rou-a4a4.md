---
version: 2
name: cross-cutting-client-integration-modules-procedure-rou-a4a4
description: "Internal env Module Adoption for Centralized Environment Configuration: Client Integration Modules Procedure Routers Consume"
metadata:
  actual:
    adr_id: a4a4c75f-f2f6-4115-a31d-845f17e71413
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

# Internal env Module Adoption for Centralized Environment Configuration: Client Integration Modules Procedure Routers Consume

Scope: applies to `packages/api/src/lib/**`, `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENV-001** SHOULD: Client integration modules and procedure routers SHOULD consume credential properties directly from the imported env module at the point of invocation to prevent stale reference caching.

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