---
version: 2
name: cross-cutting-when-integrating-external-validation-lib-db43
description: "error-code Internal Module Adoption for Standardized Error Classification: When Integrating External Validation Libraries Schema"
metadata:
  actual:
    adr_id: db43c0e4-705b-422d-b526-01efb0381a49
    status: accepted
    candidate_group_id: 3d87529b-a4d8-4737-9420-07ad10e3ad86
    evidence:
      source: deep
  scope:
    root: packages/error
    paths:
      - src/**
---

# error-code Internal Module Adoption for Standardized Error Classification: When Integrating External Validation Libraries Schema

Scope: applies to `packages/error/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ERR-001** MUST: When integrating external validation libraries with schema error definitions, consumers MUST inspect the repository lock artifact to verify and resolve the exact locked dependency version prior to implementation.
- **R-ERR-002** MUST: All error class definitions, error subclasses, and error handling utilities within the error subsystem MUST import the centralized error-code module and assign the appropriate registered error code during instance construction.
- **R-ERR-003** MUST: Configure diagnostic utilities to inspect constructor metadata and bound error codes to ensure uniform structured telemetry.

### Verify

```bash
# Locate and execute the repository static analysis and type verification suite to confirm all error classes import and reference registered error codes.
# Discover and run the package test runner to execute unit test suites covering base errors, HTTP errors, schema errors, and diagnostic utilities.
```

**Accept when:**
- Static analysis and type verification pass without unresolved error-code references across all error modules.
- Unit test suites for all specialized error classes and diagnostic utilities execute and pass with zero failures.

<enforcement>
Claude Code MUST NOT skip or defer verification. Violation handling: Pull requests containing ad-hoc inline error codes or unapproved error classifications will be blocked by continuous integration checks.
</enforcement>