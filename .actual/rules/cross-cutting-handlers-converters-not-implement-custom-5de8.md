---
version: 2
name: cross-cutting-handlers-converters-not-implement-custom-5de8
description: "Adoption of @openstatus/assertions Module for Monitor Validation and Serialization: Handlers Converters Not Implement Custom Assertion"
metadata:
  actual:
    adr_id: 5de8cf72-103a-4c57-a6b1-405bcce87e11
    status: accepted
    candidate_group_id: 18e039bc-6f1d-4be9-be61-61fcfc11a203
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Adoption of @openstatus/assertions Module for Monitor Validation and Serialization: Handlers Converters Not Implement Custom Assertion

Scope: applies to `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ADR-001** MUST_NOT: Handlers and converters MUST_NOT implement custom assertion parsing logic or bypass the validation schemas defined in @openstatus/assertions.

### Verify

```bash
# Discover and execute the project workspace test suite to verify assertion validation and parsing tests pass.
# Run the repository static analysis and type checker to confirm conformance with @openstatus/assertions types across all monitor route handlers and converters.
```

**Accept when:**
- All monitor route handlers and converters compile with shared assertion module imports.
- Type checking and assertion schema parsing tests pass with zero validation mismatches across test suites.
- Serialized monitor assertion payloads conform to the canonical assertions module representation.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>