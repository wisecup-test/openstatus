---
version: 2
name: cross-cutting-when-introducing-updating-versioned-exte-203e
description: "Adoption of base-error Internal Module for Structured Error Definitions: When Introducing Updating Versioned External Dependencies"
metadata:
  actual:
    adr_id: 203ebff8-ce77-4332-987c-5ed0fa59b501
    status: accepted
    candidate_group_id: 4689aee9-392e-481e-85d5-49d67acc913c
    evidence:
      source: deep
  scope:
    root: packages/error
    paths:
      - src/**
---

# Adoption of base-error Internal Module for Structured Error Definitions: When Introducing Updating Versioned External Dependencies

Scope: applies to `packages/error/src/**`.

These rules are ALWAYS ACTIVE for specialized error classes, error definition modules within domain/transport packages, and error mapping adapters transforming external validation failures into internal domain errors.

### Rules

- **R-ERR-001** MUST: When introducing or updating versioned external dependencies for error validation, the consumer MUST inspect the repository lock or resolution artifact to confirm the exact resolved version before implementation.
- **R-ERR-002** MUST: Construct all new specialized domain error types by extending the internal base-error abstraction and binding an error code from the internal error-code registry.
- **R-ERR-003** MUST: Route external schema validation errors through dedicated schema error adapters that normalize third-party validation issues into base error formats.

### Verify

```bash
# Discover the project dependency manifest, test execution script, type-checking, and linting scripts from the workspace
# and execute verification across error modules.
```

**Accept when:**
- All specialized error classes inherit from or encapsulate the internal base-error module and compile without interface mismatch.
- Automated verification suites confirm consistent error code mapping and metadata serialization across all specialized error definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by continuous integration pipelines and peer code review.
</enforcement>