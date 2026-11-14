---
version: 2
name: cross-cutting-input-schemas-perform-boundary-level-783a
description: "Zod Runtime Schema Validation for API Procedure Boundaries: Input Schemas Perform Boundary Level Normalizations"
metadata:
  actual:
    adr_id: 783af2c6-9d8d-4058-86ef-514f0b402cc3
    status: accepted
    candidate_group_id: 0cbb6056-fd7d-4539-b46b-da94fc5fa10f
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/**
---

# Zod Runtime Schema Validation for API Procedure Boundaries: Input Schemas Perform Boundary Level Normalizations

Scope: applies to `packages/api/src/**`.

These rules are ALWAYS ACTIVE for all public and protected procedure definitions accepting parameters from external callers.

### Rules

- **R-ZOD-001** SHOULD: Input schemas SHOULD perform boundary-level normalizations, including casing transforms and primitive coercions, within the schema pipeline prior to service invocation.

### Verify

```bash
# Discover and execute the repository build and type verification script
# Discover and execute the repository automated test runner
# Discover and run the project static analysis linter
```

**Accept when:**
- All public and protected procedure definitions declare explicit input validation schemas prior to invoking service handlers.
- Type checking and automated test suites confirm schema compilation and successful rejection of malformed payloads.
- Boundary parser rejections return structured client errors without propagating unvalidated state to domain functions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis and type checking integrated into continuous integration pipelines ensure compliance.
</enforcement>