---
version: 2
name: cross-cutting-error-definitions-deriving-base-integrat-ebd6
description: "Adoption of base-error Internal Module for Structured Error Definitions: Error Definitions Deriving Base Integrate Standardized"
metadata:
  actual:
    adr_id: ebd68d5d-63f2-456b-9820-a861d0fbb24b
    status: accepted
    candidate_group_id: 4689aee9-392e-481e-85d5-49d67acc913c
    evidence:
      source: deep
  scope:
    root: packages/error
    paths:
      - src/**
---

# Adoption of base-error Internal Module for Structured Error Definitions: Error Definitions Deriving Base Integrate Standardized

Scope: applies to `packages/error/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ERR-001** MUST: Error definitions deriving from base-error MUST integrate standardized error codes through the internal error-code module rather than arbitrary literal values.

### Verify

```bash
# Discover the project test execution script from the workspace dependency manifest and execute test validation across error modules.
# Discover and execute the project type-checking and linting verification scripts to ensure compliance with base-error interface constraints.
```

**Accept when:**
- All specialized error classes inherit from or encapsulate the internal base-error module and compile without interface mismatch.
- Automated verification suites confirm consistent error code mapping and metadata serialization across all specialized error definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>