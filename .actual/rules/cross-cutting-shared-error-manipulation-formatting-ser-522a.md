---
version: 2
name: cross-cutting-shared-error-manipulation-formatting-ser-522a
description: "Adoption of base-error Internal Module for Structured Error Definitions: Shared Error Manipulation Formatting Serialization Logic"
metadata:
  actual:
    adr_id: 522a20c4-db6d-466d-9dcc-f31f154ebad5
    status: accepted
    candidate_group_id: 4689aee9-392e-481e-85d5-49d67acc913c
    evidence:
      source: deep
  scope:
    root: packages/error
    paths:
      - src/**
---

# Adoption of base-error Internal Module for Structured Error Definitions: Shared Error Manipulation Formatting Serialization Logic

Scope: applies to `packages/error/src/**`.

These rules are ALWAYS ACTIVE for specialized error classes, error definition modules within domain and transport packages, and error mapping adapters.

### Rules

- **R-ERR-001** MUST: Shared error manipulation, formatting, and serialization logic MUST utilize the internal utils module to preserve operational consistency across error variants.
- **R-ERR-002** MUST: Construct all new specialized domain error types by extending the internal base-error abstraction and binding an error code from the internal error-code registry.
- **R-ERR-003** MUST: Route external schema validation errors through dedicated schema error adapters that normalize third-party validation issues into base error formats.

### Verify

```bash
# Discover and execute the project test execution script from the workspace dependency manifest
# Discover and execute the project type-checking and linting verification scripts
```

**Accept when:**
- All specialized error classes inherit from or encapsulate the internal base-error module and compile without interface mismatch.
- Automated verification suites confirm consistent error code mapping and metadata serialization across all specialized error definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification. All specialized error definitions must strictly conform to the base error architecture.
</enforcement>