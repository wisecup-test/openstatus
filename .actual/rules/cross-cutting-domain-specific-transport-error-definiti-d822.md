---
version: 2
name: cross-cutting-domain-specific-transport-error-definiti-d822
description: "error-code Internal Module Adoption for Standardized Error Classification: Domain Specific Transport Error Definitions Extend"
metadata:
  actual:
    adr_id: d8221cf4-77ba-42c5-a08a-c5346fff90d9
    status: accepted
    candidate_group_id: 3d87529b-a4d8-4737-9420-07ad10e3ad86
    evidence:
      source: deep
  scope:
    root: packages/error
    paths:
      - src/**
---

# error-code Internal Module Adoption for Standardized Error Classification: Domain Specific Transport Error Definitions Extend

Scope: applies to `packages/error/src/**`.

These rules are ALWAYS ACTIVE for all error class definitions, error subclasses, and error handling utilities within the error subsystem, including new specialized domain or transport error implementations.

### Rules

- **R-ERR-001** SHOULD: Domain-specific and transport-specific error definitions SHOULD extend the base error class while preserving error-code categorization.

### Verify

```bash
# Locate and execute the repository static analysis and type verification suite
# Discover and run the package test runner to execute unit test suites covering base errors, HTTP errors, schema errors, and diagnostic utilities
```

**Accept when:**
- Static analysis and type verification pass without unresolved error-code references across all error modules.
- Unit test suites for all specialized error classes and diagnostic utilities execute and pass with zero failures.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests modifying error definitions or error classification modules must pass automated static analysis and type verification suites, and are subject to mandatory peer review.
</enforcement>