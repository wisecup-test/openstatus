---
version: 2
name: cross-cutting-failed-assertion-parsing-routines-record-9daf
description: "Adoption of @openstatus/assertions Module for Monitor Validation and Serialization: Failed Assertion Parsing Routines Record Structured"
metadata:
  actual:
    adr_id: 9daf67ad-c2be-4b51-9f1b-1b837ab2078b
    status: accepted
    candidate_group_id: 18e039bc-6f1d-4be9-be61-61fcfc11a203
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Adoption of @openstatus/assertions Module for Monitor Validation and Serialization: Failed Assertion Parsing Routines Record Structured

Scope: applies to `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ADR-001** SHOULD: Failed assertion parsing routines SHOULD record structured diagnostics containing error details and input payloads to monitoring logs.

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
Claude Code MUST NOT skip or defer verification. Continuous integration static type analysis, automated pull request linting, and peer architecture review verify conformance.
</enforcement>