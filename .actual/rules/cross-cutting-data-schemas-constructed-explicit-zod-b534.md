---
version: 2
name: cross-cutting-data-schemas-constructed-explicit-zod-b534
description: "Adoption of Zod for Runtime Input and Boundary Validation: Data Schemas Constructed Explicit Zod Type"
metadata:
  actual:
    adr_id: b534770a-f2e8-450b-b007-76099b8fa342
    status: accepted
    candidate_group_id: 25160848-33da-4fdc-b666-faaa817d09fe
    evidence:
      source: deep
  scope:
    root: packages/notifications/pagerduty
    paths:
      - src/**
---

# Adoption of Zod for Runtime Input and Boundary Validation: Data Schemas Constructed Explicit Zod Type

Scope: applies to `packages/notifications/pagerduty/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST: Data schemas MUST be constructed using explicit Zod type primitives and structural combinators including z.object, z.string, and z.array to model required and optional data attributes.

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
Claude Code MUST NOT skip or defer verification. Verified by automated test suites, static analysis, and peer code reviews.
</enforcement>