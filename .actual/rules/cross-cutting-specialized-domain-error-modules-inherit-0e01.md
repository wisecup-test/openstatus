---
version: 2
name: cross-cutting-specialized-domain-error-modules-inherit-0e01
description: "Adoption of base-error Internal Module for Structured Error Definitions: Specialized Domain Error Modules Inherit Encapsulate"
metadata:
  actual:
    adr_id: 0e01a638-fbbf-4239-871a-ad54ad401307
    status: accepted
    candidate_group_id: 4689aee9-392e-481e-85d5-49d67acc913c
    evidence:
      source: deep
  scope:
    root: packages/error
    paths:
      - src/**
---

# Adoption of base-error Internal Module for Structured Error Definitions: Specialized Domain Error Modules Inherit Encapsulate

Scope: applies to `packages/error/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ERR-001** MUST: Specialized domain error modules MUST inherit from or encapsulate the internal base-error module to maintain a uniform error hierarchy and serialization contract.

### Verify

```bash
# Discover and execute project test execution script, type-checking, and linting scripts from workspace dependency manifest
```

**Accept when:**
- All specialized error classes inherit from or encapsulate the internal base-error module and compile without interface mismatch.
- Automated verification suites confirm consistent error code mapping and metadata serialization across all specialized error definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipelines, peer code reviews, and static analysis will block uncoordinated error definitions that bypass the base error module.
</enforcement>