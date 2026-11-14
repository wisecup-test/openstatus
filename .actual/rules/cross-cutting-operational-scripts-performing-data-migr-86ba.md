---
version: 2
name: cross-cutting-operational-scripts-performing-data-migr-86ba
description: "Console API Tagged Prefix Logging for Error and Warning Diagnostics: Operational Scripts Performing Data Migration Backfill"
metadata:
  actual:
    adr_id: 86badfdc-40a6-4b02-9b15-75de69bdd4b0
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

# Console API Tagged Prefix Logging for Error and Warning Diagnostics: Operational Scripts Performing Data Migration Backfill

Scope: applies to `packages/tinybird/scripts/**`, `packages/tinybird/src/**`.

These rules are ALWAYS ACTIVE for operational data backfill and migration scripts interacting with primary datastores, and client integration modules managing external analytics connectivity and safe query execution.

### Rules

- **R-OPR-001** SHOULD: Operational scripts performing data migration or backfill routines SHOULD emit warnings via console.warn when overriding threshold checks or when encountering unmapped schema entity records.

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