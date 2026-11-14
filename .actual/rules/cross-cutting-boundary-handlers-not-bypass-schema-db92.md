---
version: 2
name: cross-cutting-boundary-handlers-not-bypass-schema-db92
description: "Adoption of Zod for Runtime Input and Boundary Validation: Boundary Handlers Not Bypass Schema Parsing"
metadata:
  actual:
    adr_id: db9262b9-8bc2-46b4-a596-8d06255e761b
    status: accepted
    candidate_group_id: 25160848-33da-4fdc-b666-faaa817d09fe
    evidence:
      source: deep
  scope:
    root: packages/notifications/pagerduty
    paths:
      - src/**
---

# Adoption of Zod for Runtime Input and Boundary Validation: Boundary Handlers Not Bypass Schema Parsing

Scope: applies to `packages/notifications/pagerduty/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST_NOT: Boundary handlers MUST NOT bypass schema parsing or cast untrusted deserialized objects directly to internal interfaces without runtime validation.

### Verify

```bash
# Discover the project dependency manifest and lock artifact to identify the registered test and typecheck scripts, then execute the suite to verify that all boundary schemas compile without errors.
# Discover and run the project static analysis and linting verification suite to ensure all input ingestion boundaries invoke Zod schema parse methods on serialized payloads.
# Execute the project automated test suite to validate that malformed input payloads trigger expected validation failures and prevent execution of external client dispatches.
```

**Accept when:**
- All boundary handlers parsing serialized incoming data or configuring external client dispatches pass automated test suites and reject malformed schemas with validation errors.
- Static analysis and typecheck suites complete with zero errors across all modules declaring and parsing Zod boundary schemas.
- The resolved library version matches the documented API usage confirmed from the authoritative dependency lock artifact.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests omitting Zod validation at untrusted input boundaries will be blocked during code review.
</enforcement>