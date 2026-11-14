---
version: 2
name: cross-cutting-monitor-route-handlers-utility-functions-659d
description: "Adoption of @openstatus/assertions Module for Monitor Validation and Serialization: Monitor Route Handlers Utility Functions Instantiate"
metadata:
  actual:
    adr_id: 659dd717-dda7-4fea-8037-765a4b5ce38d
    status: accepted
    candidate_group_id: 18e039bc-6f1d-4be9-be61-61fcfc11a203
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Adoption of @openstatus/assertions Module for Monitor Validation and Serialization: Monitor Route Handlers Utility Functions Instantiate

Scope: applies to `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-MON-001** MUST: Monitor route handlers and utility functions MUST instantiate concrete assertion domain classes incorporating explicit version markers for in-memory collections.

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
Claude Code MUST NOT skip or defer verification.
</enforcement>