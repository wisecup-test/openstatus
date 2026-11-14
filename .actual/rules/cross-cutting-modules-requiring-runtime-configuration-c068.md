---
version: 2
name: cross-cutting-modules-requiring-runtime-configuration-c068
description: "Internal env Module Adoption for Centralized Environment Configuration: Modules Requiring Runtime Configuration Third Party"
metadata:
  actual:
    adr_id: c068bd18-74cc-4b2d-9b8e-e12cb98e0915
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

# Internal env Module Adoption for Centralized Environment Configuration: Modules Requiring Runtime Configuration Third Party

Scope: applies to `packages/api/src/lib/**`, `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for all modules requiring runtime configuration, third-party service identifiers, or secret credentials within the API package.

### Rules

- **R-ENV-001** MUST: All modules requiring runtime configuration, third-party service identifiers, or secret credentials MUST import and consume values exclusively through the internal env module.

### Verify

```bash
# Discover and execute the project test runner covering API routers and client libraries
# Discover and execute static analysis and linting verification scripts to check for unauthorized ambient environment accesses
```

**Accept when:**
- All API routers and integration client modules import configuration properties exclusively from the central env module.
- Test execution passes without errors resulting from missing or unvalidated environment credentials.
- Static verification confirms absence of direct ambient process environment access outside permitted bootstrap exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration automated static analysis checks, test suite runs, and peer code reviews enforce compliance.
</enforcement>