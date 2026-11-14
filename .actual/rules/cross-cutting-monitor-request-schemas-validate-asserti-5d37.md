---
version: 2
name: cross-cutting-monitor-request-schemas-validate-asserti-5d37
description: "Adoption of @openstatus/assertions Module for Monitor Validation and Serialization: Monitor Request Schemas Validate Assertion Definitions"
metadata:
  actual:
    adr_id: 5d374ce5-3074-41e4-b4f9-10590e12dd63
    status: accepted
    candidate_group_id: 18e039bc-6f1d-4be9-be61-61fcfc11a203
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Adoption of @openstatus/assertions Module for Monitor Validation and Serialization: Monitor Request Schemas Validate Assertion Definitions

Scope: applies to `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-MON-001** MUST: Monitor request schemas MUST validate assertion definitions through @openstatus/assertions parsing functions before persisting or processing monitor configurations.

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
Claude Code MUST NOT skip or defer verification. Continuous integration static type analysis, test suite execution, automated pull request linting, and import boundary verification must be enforced.
</enforcement>