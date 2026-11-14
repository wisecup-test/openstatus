---
version: 2
name: cross-cutting-schemas-define-fallback-default-values-a110
description: "Zod Schema Validation for External Provider Payloads: Schemas Define Fallback Default Values Transformations"
metadata:
  actual:
    adr_id: a110fc94-0d9d-48dd-b044-db527371cbe0
    status: accepted
    candidate_group_id: 47a971ad-1e20-4235-b2c9-cbdf09abe622
    evidence:
      source: deep
  scope:
    root: packages/importers
    paths:
      - src/providers/**
---

# Zod Schema Validation for External Provider Payloads: Schemas Define Fallback Default Values Transformations

Scope: applies to `packages/importers/src/providers/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MAY: Schemas MAY define fallback default values or transformations to convert heterogeneous external identifiers into standardized internal formats.

### Verify

```bash
# Discover and run the project test suite to ensure all provider schema parsing tests pass against simulated external payloads.
# Discover and run the project type checker and static analysis suite to verify strict type inference from parsed schemas.
```

**Accept when:**
- All integration test suites pass with validated external payload assertions.
- Static type checking confirms all downstream consumers receive validated and inferred data types without manual type casting.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>