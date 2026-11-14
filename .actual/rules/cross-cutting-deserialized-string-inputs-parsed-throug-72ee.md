---
version: 2
name: cross-cutting-deserialized-string-inputs-parsed-throug-72ee
description: "Adoption of Zod for Runtime Input and Boundary Validation: Deserialized String Inputs Parsed Through Parse"
metadata:
  actual:
    adr_id: 72eea71a-403c-4183-b56d-1054d6ee555d
    status: accepted
    candidate_group_id: 25160848-33da-4fdc-b666-faaa817d09fe
    evidence:
      source: deep
  scope:
    root: packages/notifications/pagerduty
    paths:
      - src/**
---

# Adoption of Zod for Runtime Input and Boundary Validation: Deserialized String Inputs Parsed Through Parse

Scope: applies to `packages/notifications/pagerduty/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST: Deserialized string inputs MUST be parsed through the parse method of a defined Zod schema before accessing nested properties in business logic.

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
Claude Code MUST NOT skip or defer verification.
</enforcement>