---
version: 2
name: cross-cutting-assertion-data-structures-payload-serial-b8d8
description: "Adopt Zod for Runtime Assertion Schema Validation and Parsing: Assertion Data Structures Payload Serialization Boundaries"
metadata:
  actual:
    adr_id: b8d87381-9423-4c5f-aa12-f9d39f3d5554
    status: accepted
    candidate_group_id: 2433e36a-68e4-4bbc-93f7-a891b38dda85
    evidence:
      source: deep
  scope:
    root: packages/assertions
    paths:
      - src/**
---

# Adopt Zod for Runtime Assertion Schema Validation and Parsing: Assertion Data Structures Payload Serialization Boundaries

Scope: applies to `packages/assertions/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST: All assertion data structures and payload serialization boundaries MUST be defined and parsed using zod runtime schemas.
- **R-ZOD-002** MANDATORY: Execute lock-version grounding (find manifest, identify build tool, inspect lock artifact, look up official docs for exact version, confirm APIs exist) before writing code that uses a versioned library.
- **R-ZOD-003** MANDATORY: Discover the test runner from the project manifest and execute the test suite covering assertion schema validation and serialization.
- **R-ZOD-004** MANDATORY: Discover the static analysis tool from the project configuration and verify type compatibility between inferred schema types and assertion handlers.

### Verify

```bash
# Discover and run test suite covering assertion schema validation and serialization
# (e.g. npm test or equivalent based on project manifest)
# Discover and run static type checking / analysis
# (e.g. npx tsc or equivalent based on project configuration)
```

**Accept when:**
- All assertion payload parsing tests pass without schema validation regressions.
- Static type checking completes with zero type errors across assertion schema definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification. All assertion contracts must export and consume zod schemas; pull requests ingesting unvalidated assertion structures or using manual type casting without schema parsing will be rejected.
</enforcement>