---
version: 2
name: cross-cutting-monitor-assertion-parsing-typed-model-e0b4
description: "Adoption of @openstatus/assertions Module for Monitor Validation and Serialization: Monitor Assertion Parsing Typed Model Instantiation"
metadata:
  actual:
    adr_id: e0b46901-77f4-43e3-92cd-d17325bb8852
    status: accepted
    candidate_group_id: 18e039bc-6f1d-4be9-be61-61fcfc11a203
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Adoption of @openstatus/assertions Module for Monitor Validation and Serialization: Monitor Assertion Parsing Typed Model Instantiation

Scope: applies to `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for all monitor configuration ingestion, parsing, conversion, and persistence operations.

### Rules

- **R-AST-001** MUST: All monitor assertion parsing, typed model instantiation, and persistence serialization MUST standardize on the @openstatus/assertions module.
- **R-AST-002** MUST: Instantiate assertion instances using appropriate domain constructors before appending to monitor configuration collections.
- **R-AST-003** MUST: Ensure all database persistence operations invoke serialize from the assertions module prior to record storage.
- **R-AST-004** MUST: Capture and log raw payload fragments alongside parsing failure diagnostics to assist monitor debugging.

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
Claude Code MUST NOT skip or defer verification. Verified by continuous integration static type analysis, test suite execution, automated pull request linting, and peer architecture review.
</enforcement>