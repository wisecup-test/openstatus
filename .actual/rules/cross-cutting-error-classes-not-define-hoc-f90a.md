---
version: 2
name: cross-cutting-error-classes-not-define-hoc-f90a
description: "error-code Internal Module Adoption for Standardized Error Classification: Error Classes Not Define Hoc String"
metadata:
  actual:
    adr_id: f90a23d7-01b9-4684-a0c6-5eda4c908019
    status: accepted
    candidate_group_id: 3d87529b-a4d8-4737-9420-07ad10e3ad86
    evidence:
      source: deep
  scope:
    root: packages/error
    paths:
      - src/**
---

# error-code Internal Module Adoption for Standardized Error Classification: Error Classes Not Define Hoc String

Scope: applies to `packages/error/src/**`.

These rules are ALWAYS ACTIVE for all error class definitions, error subclasses, and error handling utilities within the error subsystem.

### Rules

- **R-ERR-001** MUST_NOT: Error classes MUST NOT define ad-hoc string literals or numeric codes for error identification outside the centralized error-code module.

### Verify

```bash
# Locate and execute the repository static analysis and type verification suite to confirm all error classes import and reference registered error codes.
# Discover and run the package test runner to execute unit test suites covering base errors, HTTP errors, schema errors, and diagnostic utilities.
```

**Accept when:**
- Static analysis and type verification pass without unresolved error-code references across all error modules.
- Unit test suites for all specialized error classes and diagnostic utilities execute and pass with zero failures.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>