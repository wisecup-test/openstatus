---
version: 2
name: cross-cutting-error-classes-subclasses-diagnostic-util-b5e9
description: "error-code Internal Module Adoption for Standardized Error Classification: Error Classes Subclasses Diagnostic Utilities Within"
metadata:
  actual:
    adr_id: b5e959df-8b43-4907-a3ee-664c50c1c19d
    status: accepted
    candidate_group_id: 3d87529b-a4d8-4737-9420-07ad10e3ad86
    evidence:
      source: deep
  scope:
    root: packages/error
    paths:
      - src/**
---

# error-code Internal Module Adoption for Standardized Error Classification: Error Classes Subclasses Diagnostic Utilities Within

Scope: applies to `packages/error/src/**`.

These rules are ALWAYS ACTIVE for all error class definitions, error subclasses, error handling utilities, and specialized domain or transport error implementations within the error subsystem.

### Rules

- **R-ERR-001** MUST: All error classes, subclasses, and diagnostic utilities within the error subsystem MUST import and bind their error classification identities directly to the centralized error-code internal module.

### Verify

```bash
# Locate and execute repository static analysis and type verification suite
# Discover and run package test runner to execute unit test suites covering errors and utilities
```

**Accept when:**
- Static analysis and type verification pass without unresolved error-code references across all error modules.
- Unit test suites for all specialized error classes and diagnostic utilities execute and pass with zero failures.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipelines and automated static analysis block pull requests containing ad-hoc inline error codes or unapproved error classifications.
</enforcement>