---
version: 2
name: cross-cutting-diagnostic-logging-calls-not-emit-a262
description: "Console API Tagged Prefix Logging for Error and Warning Diagnostics: Diagnostic Logging Calls Not Emit Sensitive"
metadata:
  actual:
    adr_id: a2628d50-6ab0-4140-b764-a894085317c4
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

# Console API Tagged Prefix Logging for Error and Warning Diagnostics: Diagnostic Logging Calls Not Emit Sensitive

Scope: applies to `packages/tinybird/scripts/**`, `packages/tinybird/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-LOG-001** MUST_NOT: Diagnostic logging calls MUST_NOT emit sensitive authentication secrets, private tokens, or unmasked database connection credentials to standard error or standard output streams.

### Verify

```bash
# Discover and execute the repository static analysis and linting scripts from the dependency manifest
# Discover and execute the package test suite from the repository manifest
```

**Accept when:**
- Static analysis validates that all logging calls use designated console methods with standardized component prefix strings.
- Unit and integration tests verify that error fallback paths execute and emit diagnostic error messages without crashing.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>