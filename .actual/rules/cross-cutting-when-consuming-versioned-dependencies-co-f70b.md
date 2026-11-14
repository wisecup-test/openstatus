---
version: 2
name: cross-cutting-when-consuming-versioned-dependencies-co-f70b
description: "Zod Schema Definition and Runtime Validation: When Consuming Versioned Dependencies Consumer Inspect"
metadata:
  actual:
    adr_id: f70b0598-2a8b-40f1-9cc3-999e9f3b96bf
    status: accepted
    candidate_group_id: b8eea867-4633-4dd5-b6aa-39b93daf18b4
    evidence:
      source: deep
  scope:
    root: packages/notifications/telegram
    paths:
      - src/**
---

# Zod Schema Definition and Runtime Validation: When Consuming Versioned Dependencies Consumer Inspect

Scope: applies to `packages/notifications/telegram/src/**`.

These rules are ALWAYS ACTIVE for channel configuration schemas and external payload parsing boundaries within integration packages.

### Rules

- **R-ZOD-001** MUST: When consuming versioned dependencies, the consumer MUST inspect the repository lock or resolution artifact and resolve the exact locked version before implementation.
- **R-ZOD-002** MUST: Define channel configurations and external payload parsing boundaries using Zod declarative schema builders.
- **R-ZOD-003** MUST: Infer TypeScript types directly from schemas rather than manually writing independent interface definitions.
- **R-ZOD-004** MUST: Define reusable schemas as module-level constants to prevent redundant allocation during payload processing.

### Verify

```bash
# Discover the project test runner from the root manifest and run unit tests covering schema validation.
# Discover the repository type check script and verify that schema inference types align with expected contract interfaces.
```

**Accept when:**
- All unit tests exercising schema parsing and boundary rejection pass successfully.
- Static type analysis confirms zero type mismatch errors between inferred schema types and boundary consumer signatures.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory and enforced by automated test suite execution and peer code reviews.
</enforcement>