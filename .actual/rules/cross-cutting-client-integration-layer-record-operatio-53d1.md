---
version: 2
name: cross-cutting-client-integration-layer-record-operatio-53d1
description: "Console API Tagged Prefix Logging for Error and Warning Diagnostics: Client Integration Layer Record Operational Anomalies"
metadata:
  actual:
    adr_id: 53d1f689-701a-4a8e-a8fa-b21f7c594329
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

# Console API Tagged Prefix Logging for Error and Warning Diagnostics: Client Integration Layer Record Operational Anomalies

Scope: applies to `packages/tinybird/scripts/**`, `packages/tinybird/src/**`.

These rules are ALWAYS ACTIVE for client integration modules managing external analytics connectivity, safe query execution, and operational data backfill/migration scripts interacting with primary datastores.

### Rules

- **R-LOG-001** MUST: The client integration layer MUST record operational anomalies, unconfigured authentication credentials, and query execution failures using standard Console API error and warning methods prefixed with a bracketed component identifier.
- **R-LOG-002** MUST: Ensure safe fallback routines attach the caught error object as a secondary parameter rather than concatenating it as a string to preserve stack traces.
- **R-LOG-003** MUST: Standardize bracketed domain prefixes across all client diagnostic messages to maintain uniform log message parsing.

### Verify

```bash
# Discover and execute the repository static analysis and linting scripts from the dependency manifest
# to verify console logging compliance across client packages.
# Discover and execute the package test suite from the repository manifest to verify that
# fallback error handling emits diagnostic messages as expected.
```

**Accept when:**
- Static analysis validates that all logging calls use designated console methods with standardized component prefix strings.
- Unit and integration tests verify that error fallback paths execute and emit diagnostic error messages without crashing.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>