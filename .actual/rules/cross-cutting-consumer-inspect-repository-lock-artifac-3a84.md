---
version: 2
name: cross-cutting-consumer-inspect-repository-lock-artifac-3a84
description: "Adoption of @openstatus/assertions Module for Monitor Validation and Serialization: Consumer Inspect Repository Lock Artifact Verify"
metadata:
  actual:
    adr_id: 3a84f3bf-1dbe-4ade-8444-468f749a41e8
    status: accepted
    candidate_group_id: 18e039bc-6f1d-4be9-be61-61fcfc11a203
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Adoption of @openstatus/assertions Module for Monitor Validation and Serialization: Consumer Inspect Repository Lock Artifact Verify

Scope: applies to `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-001** MUST: The consumer MUST inspect the repository lock artifact and verify the exact resolved version of the assertions module before introducing or updating assertion implementations.

### Verify

```bash
# Discover and execute the project workspace test suite to verify assertion validation and parsing tests pass
# Run the repository static analysis and type checker to confirm conformance with @openstatus/assertions types across all monitor route handlers and converters
```

**Accept when:**
- All monitor route handlers and converters compile with shared assertion module imports.
- Type checking and assertion schema parsing tests pass with zero validation mismatches across test suites.
- Serialized monitor assertion payloads conform to the canonical assertions module representation.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration static type analysis, automated pull request linting, and peer architecture reviews enforce these rules.
</enforcement>