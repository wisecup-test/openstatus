---
version: 2
name: cross-cutting-components-defining-runtime-data-contrac-5289
description: "Zod Schema Definition and Runtime Validation: Components Defining Runtime Data Contracts External"
metadata:
  actual:
    adr_id: 528996db-33e2-4679-8e61-72f28a1802de
    status: accepted
    candidate_group_id: b8eea867-4633-4dd5-b6aa-39b93daf18b4
    evidence:
      source: deep
  scope:
    root: packages/notifications/telegram
    paths:
      - src/**
---

# Zod Schema Definition and Runtime Validation: Components Defining Runtime Data Contracts External

Scope: applies to `packages/notifications/telegram/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST: Components defining runtime data contracts and external channel configurations MUST declare schemas using Zod.

### Verify

```bash
# Discover the project test runner from the root manifest and run unit tests covering schema validation.
# Discover the repository type check script and verify that schema inference types align with expected contract interfaces.
```

**Accept when:**
- All unit tests exercising schema parsing and boundary rejection pass successfully.
- Static type analysis confirms zero type mismatch errors between inferred schema types and boundary consumer signatures.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>