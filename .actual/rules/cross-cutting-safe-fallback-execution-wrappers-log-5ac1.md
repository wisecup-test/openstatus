---
version: 2
name: cross-cutting-safe-fallback-execution-wrappers-log-5ac1
description: "Console API Tagged Prefix Logging for Error and Warning Diagnostics: Safe Fallback Execution Wrappers Log Specific"
metadata:
  actual:
    adr_id: 5ac12760-2040-4e6c-a7e5-bd1444aa50ee
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

# Console API Tagged Prefix Logging for Error and Warning Diagnostics: Safe Fallback Execution Wrappers Log Specific

Scope: applies to `packages/tinybird/scripts/**`, `packages/tinybird/src/**`.

These rules are ALWAYS ACTIVE for client integration modules managing external analytics connectivity, safe query execution, and operational data backfill/migration scripts interacting with primary datastores.

### Rules

- **R-SF-001** MUST: Safe fallback execution wrappers MUST log the specific operation identifier and original error payload via console.error prior to returning safe empty fallback datasets to callers.

### Verify

```bash
# Discover the repository static analysis and linting scripts from the dependency manifest and execute them to verify console logging compliance across client packages.
# Discover and execute the package test suite from the repository manifest to verify that fallback error handling emits diagnostic messages as expected.
```

**Accept when:**
- Static analysis validates that all logging calls use designated console methods with standardized component prefix strings.
- Unit and integration tests verify that error fallback paths execute and emit diagnostic error messages without crashing.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>