---
version: 2
name: cross-cutting-engineering-team-inspect-repository-lock-c5c1
description: "Adopt Zod for Runtime Assertion Schema Validation and Parsing: Engineering Team Inspect Repository Lock Artifact"
metadata:
  actual:
    adr_id: c5c117c5-ea8e-401c-a66c-2f970f3daa65
    status: accepted
    candidate_group_id: 2433e36a-68e4-4bbc-93f7-a891b38dda85
    evidence:
      source: deep
  scope:
    root: packages/assertions
    paths:
      - src/**
---

# Adopt Zod for Runtime Assertion Schema Validation and Parsing: Engineering Team Inspect Repository Lock Artifact

Scope: applies to `packages/assertions/src/**`.

These rules are ALWAYS ACTIVE for all assertion payload definitions, structural type declarations, deserialization, and ingestion boundaries handling serialized assertion data.

### Rules

- **R-ZOD-001** MUST: Inspect the repository lock artifact to determine the exact resolved dependency version and verify that all referenced schema APIs exist in that version documentation prior to implementation.
- **R-ZOD-002** MUST: Ensure heterogeneous assertion payloads transmitted or stored as serialized data have guaranteed structural conformance at ingestion boundaries using schema parsing.
- **R-ZOD-003** MUST: Assemble composite assertion schemas from atomic field validators using discriminated object schemas keyed on an assertion type attribute.
- **R-ZOD-004** MUST: Execute schema parsing immediately upon deserializing serialized string data to catch structural violations before domain processing occurs.
- **R-ZOD-005** MUST: Implement structured error handling around boundary parsing invocations to handle malformed payloads without unhandled runtime terminations.

### Verify

```bash
# Discover the test runner from the project manifest and execute the test suite covering assertion schema validation and serialization
# Discover the static analysis tool from the project configuration and verify type compatibility between inferred schema types and assertion handlers
```

**Accept when:**
- All assertion payload parsing tests pass without schema validation regressions.
- Static type checking completes with zero type errors across assertion schema definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated test suites execute schema parsing validation across serialized assertion payloads.
</enforcement>