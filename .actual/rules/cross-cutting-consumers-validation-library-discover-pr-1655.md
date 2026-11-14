---
version: 2
name: cross-cutting-consumers-validation-library-discover-pr-1655
description: "Adoption of Zod for Runtime Input and Boundary Validation: Consumers Validation Library Discover Project Dependency"
metadata:
  actual:
    adr_id: 1655c2d9-6693-4bbc-a378-cd3ba9bdd7ae
    status: accepted
    candidate_group_id: 25160848-33da-4fdc-b666-faaa817d09fe
    evidence:
      source: deep
  scope:
    root: packages/notifications/pagerduty
    paths:
      - src/**
---

# Adoption of Zod for Runtime Input and Boundary Validation: Consumers Validation Library Discover Project Dependency

Scope: applies to `packages/notifications/pagerduty/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST: Consumers of the validation library MUST discover the project dependency lock artifact and verify the exact resolved version against public documentation before implementing or updating schema parsing routines.

### Verify

```bash
# Discover the project dependency manifest and lock artifact to identify registered test/typecheck scripts
# Run the suite to verify boundary schemas compile without errors
# Discover and run static analysis/linting verification to ensure input ingestion boundaries invoke Zod schema parse methods
# Execute automated test suite to validate malformed input payloads trigger expected validation failures
```

**Accept when:**
- All boundary handlers parsing serialized incoming data or configuring external client dispatches pass automated test suites and reject malformed schemas with validation errors.
- Static analysis and typecheck suites complete with zero errors across all modules declaring and parsing Zod boundary schemas.
- The resolved library version matches the documented API usage confirmed from the authoritative dependency lock artifact.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>