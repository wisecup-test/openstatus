---
version: 2
name: cross-cutting-data-transformation-validation-routines-ffb0
description: "Zod Schema Definition and Runtime Validation: Data Transformation Validation Routines Execute Parsing"
metadata:
  actual:
    adr_id: ffb064ca-557e-4bc9-ac06-659a4233300d
    status: accepted
    candidate_group_id: b8eea867-4633-4dd5-b6aa-39b93daf18b4
    evidence:
      source: deep
  scope:
    root: packages/notifications/telegram
    paths:
      - src/**
---

# Zod Schema Definition and Runtime Validation: Data Transformation Validation Routines Execute Parsing

Scope: applies to `packages/notifications/telegram/src/**`.

These rules are ALWAYS ACTIVE for channel configuration schemas and external payload parsing boundaries within integration packages.

### Rules

- **R-ZOD-001** SHOULD: Data transformation and validation routines execute parsing at boundary ingress before passing validated structures to domain handlers.
- **R-ZOD-002** MANDATORY: Define reusable schemas as module-level constants to prevent redundant allocation during payload processing.
- **R-ZOD-003** MANDATORY: Infer TypeScript types directly from schemas rather than manually writing independent interface definitions.
- **R-ZOD-004** MANDATORY: Execute lock-version grounding (find manifest, identify build tool, inspect lock artifact, look up official documentation, confirm API existence, re-run per dependency at point of use) before writing code that uses a versioned library.

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