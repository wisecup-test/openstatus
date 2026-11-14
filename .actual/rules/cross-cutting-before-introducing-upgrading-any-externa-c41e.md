---
version: 2
name: cross-cutting-before-introducing-upgrading-any-externa-c41e
description: "Console API Tagged Prefix Logging for Error and Warning Diagnostics: Before Introducing Upgrading Any External Observability"
metadata:
  actual:
    adr_id: c41e1393-c04a-4603-81f2-69891131ea44
    status: accepted
    candidate_group_id: 36d6268e-35a8-4f3e-88ed-9486d899106e
    evidence:
      source: deep
  scope:
    root: packages/tinybird
    paths:
      - scripts/**
      - src/**
---

# Console API Tagged Prefix Logging for Error and Warning Diagnostics: Before Introducing Upgrading Any External Observability

Scope: applies to `packages/tinybird/scripts/**`, `packages/tinybird/src/**`.

These rules are ALWAYS ACTIVE for client integration modules managing external analytics connectivity, safe query execution, operational data backfill, and migration scripts interacting with primary datastores.

### Rules

- **R-OBS-001** MUST: Before introducing or upgrading any external observability or logging dependency, the consumer MUST inspect the repository lock artifact to determine the exact resolved dependency version.

### Verify

```bash
# Discover and execute static analysis and linting scripts from the dependency manifest
# Discover and execute the package test suite from the repository manifest
```

**Accept when:**
- Static analysis validates that all logging calls use designated console methods with standardized component prefix strings.
- Unit and integration tests verify that error fallback paths execute and emit diagnostic error messages without crashing.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated code review and peer review verify adherence.
</enforcement>