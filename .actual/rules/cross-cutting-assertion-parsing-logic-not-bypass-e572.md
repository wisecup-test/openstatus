---
version: 2
name: cross-cutting-assertion-parsing-logic-not-bypass-e572
description: "Adopt Zod for Runtime Assertion Schema Validation and Parsing: Assertion Parsing Logic Not Bypass Runtime"
metadata:
  actual:
    adr_id: e57220c9-074b-47b6-aff7-1734aa7451b1
    status: accepted
    candidate_group_id: 2433e36a-68e4-4bbc-93f7-a891b38dda85
    evidence:
      source: deep
  scope:
    root: packages/assertions
    paths:
      - src/**
---

# Adopt Zod for Runtime Assertion Schema Validation and Parsing: Assertion Parsing Logic Not Bypass Runtime

Scope: applies to `packages/assertions/src/**`.

These rules are ALWAYS ACTIVE for all assertion payload definitions, structural type declarations, and deserialization/ingestion boundaries handling serialized assertion data.

### Rules

- **R-ZOD-001** MUST_NOT: Assertion parsing logic MUST_NOT bypass runtime schema validation by casting raw deserialized objects directly to domain interfaces.

### Verify

```bash
# Discover the test runner from the project manifest and execute the test suite covering assertion schema validation and serialization.
# Discover the static analysis tool from the project configuration and verify type compatibility between inferred schema types and assertion handlers.
```

**Accept when:**
- All assertion payload parsing tests pass without schema validation regressions.
- Static type checking completes with zero type errors across assertion schema definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification. All assertion contracts must export and consume zod schemas, and pull requests ingesting unvalidated assertion structures will be rejected.
</enforcement>