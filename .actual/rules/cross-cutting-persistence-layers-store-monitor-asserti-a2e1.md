---
version: 2
name: cross-cutting-persistence-layers-store-monitor-asserti-a2e1
description: "Adoption of @openstatus/assertions Module for Monitor Validation and Serialization: Persistence Layers Store Monitor Assertion Collections"
metadata:
  actual:
    adr_id: a2e1e326-250b-4089-bcc8-549595421e1b
    status: accepted
    candidate_group_id: 18e039bc-6f1d-4be9-be61-61fcfc11a203
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Adoption of @openstatus/assertions Module for Monitor Validation and Serialization: Persistence Layers Store Monitor Assertion Collections

Scope: applies to `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-20-001** MUST: Persistence layers MUST store monitor assertion collections using the serialize utility from @openstatus/assertions rather than unformatted object stringification.

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