---
version: 2
name: cross-cutting-specialized-error-modules-not-bypass-55c6
description: "Adoption of base-error Internal Module for Structured Error Definitions: Specialized Error Modules Not Bypass Base"
metadata:
  actual:
    adr_id: 55c64bd6-19ad-4ef8-8721-1258c3295885
    status: accepted
    candidate_group_id: 4689aee9-392e-481e-85d5-49d67acc913c
    evidence:
      source: deep
  scope:
    root: packages/error
    paths:
      - src/**
---

# Adoption of base-error Internal Module for Structured Error Definitions: Specialized Error Modules Not Bypass Base

Scope: applies to `packages/error/src/**`.

These rules are ALWAYS ACTIVE for all specialized error classes and error definition modules within domain and transport packages, as well as error mapping adapters.

### Rules

- **R-ERR-001** MUST_NOT: Specialized error modules MUST NOT bypass the base-error foundation to emit unformatted, unstructured runtime errors across module boundaries.

### Verify

```bash
# Discover the project test execution script from the workspace dependency manifest and execute test validation across error modules.
# Discover and execute the project type-checking and linting verification scripts to ensure compliance with base-error interface constraints.
```

**Accept when:**
- All specialized error classes inherit from or encapsulate the internal base-error module and compile without interface mismatch.
- Automated verification suites confirm consistent error code mapping and metadata serialization across all specialized error definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is enforced by continuous integration pipelines executing static analysis and automated unit test suites on all pull requests, as well as peer code review.
</enforcement>