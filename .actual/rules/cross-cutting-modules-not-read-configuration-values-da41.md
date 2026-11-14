---
version: 2
name: cross-cutting-modules-not-read-configuration-values-da41
description: "Internal env Module Adoption for Centralized Environment Configuration: Modules Not Read Configuration Values Secret"
metadata:
  actual:
    adr_id: da416714-8951-46ab-8243-514b01caf61a
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

# Internal env Module Adoption for Centralized Environment Configuration: Modules Not Read Configuration Values Secret

Scope: applies to `packages/api/src/lib/**`, `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for API routers, procedure definitions, and client integration modules that interact with external services or depend on runtime configuration parameters.

### Rules

- **R-ENV-001** MUST_NOT: Modules MUST NOT read configuration values, secret tokens, or service endpoints directly from the ambient runtime process environment object, with exceptions reserved strictly for foundational runtime environment detection.
- **R-ENV-002** MUST: Locate the internal environment module in the API package directory structure and inspect the exported configuration object contract.
- **R-ENV-003** MUST: When introducing new external service integrations, define and export corresponding credential and endpoint properties in the central environment module prior to consuming them in routers or client libraries.

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
Claude Code MUST NOT skip or defer verification. Violation handling blocks merging of pull requests containing direct ambient process environment variable reads outside permitted bootstrap exceptions.
</enforcement>