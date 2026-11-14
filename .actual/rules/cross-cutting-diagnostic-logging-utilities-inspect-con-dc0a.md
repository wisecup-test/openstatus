---
version: 2
name: cross-cutting-diagnostic-logging-utilities-inspect-con-dc0a
description: "error-code Internal Module Adoption for Standardized Error Classification: Diagnostic Logging Utilities Inspect Constructor Identities"
metadata:
  actual:
    adr_id: dc0a9514-6beb-48aa-8e0b-b1b353cc812e
    status: accepted
    candidate_group_id: 3d87529b-a4d8-4737-9420-07ad10e3ad86
    evidence:
      source: deep
  scope:
    root: packages/error
    paths:
      - src/**
---

# error-code Internal Module Adoption for Standardized Error Classification: Diagnostic Logging Utilities Inspect Constructor Identities

Scope: applies to `packages/error/src/**`.

These rules are ALWAYS ACTIVE for all error class definitions, error subclasses, error handling utilities, and specialized domain or transport error implementations within the error subsystem.

### Rules

- **R-ERR-001** SHOULD: Diagnostic and logging utilities SHOULD inspect constructor identities and bound error-code properties to categorize runtime failures.

### Verify

```bash
# Locate and execute repository static analysis and type verification suite
# Discover and run the package test runner for error subsystem units
```

**Accept when:**
- Static analysis and type verification pass without unresolved error-code references across all error modules.
- Unit test suites for all specialized error classes and diagnostic utilities execute and pass with zero failures.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests modifying error definitions or error classification modules must pass automated static analysis and type verification.
</enforcement>