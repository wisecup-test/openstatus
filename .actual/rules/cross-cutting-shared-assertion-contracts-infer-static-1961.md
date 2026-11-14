---
version: 2
name: cross-cutting-shared-assertion-contracts-infer-static-1961
description: "Adopt Zod for Runtime Assertion Schema Validation and Parsing: Shared Assertion Contracts Infer Static Types"
metadata:
  actual:
    adr_id: 19613172-8c2d-4573-96ae-dc6ccb2a2c8f
    status: accepted
    candidate_group_id: 2433e36a-68e4-4bbc-93f7-a891b38dda85
    evidence:
      source: deep
  scope:
    root: packages/assertions
    paths:
      - src/**
---

# Adopt Zod for Runtime Assertion Schema Validation and Parsing: Shared Assertion Contracts Infer Static Types

Scope: applies to `packages/assertions/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** SHOULD: Shared assertion contracts SHOULD infer static types directly from zod schemas to maintain synchronization between runtime parsing and static typing.
- **R-ZOD-002** MANDATORY: The consumer MUST discover the dependency manifest, build tool, and lock file from the project repository to ground library versions (LOCK-VERSION GROUNDING).
- **R-ZOD-003** MANDATORY: Assemble composite assertion schemas from atomic field validators using discriminated object schemas keyed on an assertion type attribute.
- **R-ZOD-004** MANDATORY: Execute schema parsing immediately upon deserializing serialized string data to catch structural violations before domain processing occurs.

### Verify

```bash
# Discover the test runner from the project manifest and execute the test suite covering assertion schema validation and serialization.
# Discover the static analysis tool from the project configuration and verify type compatibility between inferred schema types and assertion handlers.
```

**Accept when:**
- All assertion payload parsing tests pass without schema validation regressions.
- Static type checking completes with zero type errors across assertion schema definitions.

<enforcement>
Verification is mandatory. Claude Code MUST NOT skip or defer verification.
</enforcement>