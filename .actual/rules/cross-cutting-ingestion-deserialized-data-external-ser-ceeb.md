---
version: 2
name: cross-cutting-ingestion-deserialized-data-external-ser-ceeb
description: "Adopt Zod for Runtime Assertion Schema Validation and Parsing: Ingestion Deserialized Data External Serialized Representations"
metadata:
  actual:
    adr_id: ceebdb5f-72af-4d5e-9125-516ef669adfe
    status: accepted
    candidate_group_id: 2433e36a-68e4-4bbc-93f7-a891b38dda85
    evidence:
      source: deep
  scope:
    root: packages/assertions
    paths:
      - src/**
---

# Adopt Zod for Runtime Assertion Schema Validation and Parsing: Ingestion Deserialized Data External Serialized Representations

Scope: applies to `packages/assertions/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST: Ingestion of deserialized data from external or serialized representations MUST execute schema validation via the parse method prior to domain processing.

### Verify

```bash
# Discover the test runner from the project manifest and execute the test suite covering assertion schema validation and serialization.
# Discover the static analysis tool from the project configuration and verify type compatibility between inferred schema types and assertion handlers.
```

**Accept when:**
- All assertion payload parsing tests pass without schema validation regressions.
- Static type checking completes with zero type errors across assertion schema definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated test suites and code review enforcement.
</enforcement>