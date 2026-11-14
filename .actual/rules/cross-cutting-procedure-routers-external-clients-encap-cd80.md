---
version: 2
name: cross-cutting-procedure-routers-external-clients-encap-cd80
description: "Internal env Module Adoption for Centralized Environment Configuration: Procedure Routers External Clients Encapsulate Calls"
metadata:
  actual:
    adr_id: cd80ac6d-0fa0-4d59-ae02-0eab654d371d
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

# Internal env Module Adoption for Centralized Environment Configuration: Procedure Routers External Clients Encapsulate Calls

Scope: applies to `packages/api/src/lib/**`, `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for API routers, procedure definitions, and client integration modules that interact with external services or depend on runtime configuration parameters.

### Rules

- **R-ENV-001** MAY: Procedure routers and external clients MAY encapsulate calls to the env module within helper integration wrappers to isolate external endpoint constructions from router handlers.

### Verify

```bash
# Discover and run the project test suite covering API routers and client libraries
# Discover and execute static analysis and linting scripts to verify no unauthorized ambient environment accesses exist
```

**Accept when:**
- All API routers and integration client modules import configuration properties exclusively from the central env module.
- Test execution passes without errors resulting from missing or unvalidated environment credentials.
- Static verification confirms absence of direct ambient process environment access outside permitted bootstrap exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>