---
version: 2
name: cross-cutting-validation-schemas-domain-entities-expli-3975
description: "Zod Schema Definition and Runtime Input Validation: Validation Schemas Domain Entities Explicitly Constrain"
metadata:
  actual:
    adr_id: 39758c38-f334-46e0-8f45-16f1cc26710a
    status: accepted
    candidate_group_id: 74d602d4-ffd9-4e37-b923-9b6d92d150fb
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - env.mjs
      - script/**
      - src/schema/**
---

# Zod Schema Definition and Runtime Input Validation: Validation Schemas Domain Entities Explicitly Constrain

Scope: applies to `packages/db/env.mjs`, `packages/db/script/**`, `packages/db/src/schema/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST: Validation schemas for domain entities MUST explicitly constrain primitive types, string formats, numeric bounds, and enumerated value sets rather than accepting unconstrained data types.

### Verify

```bash
# Discover and execute the repository type-check script to validate schema definitions and type inference
# Discover and execute the project test runner against the schema validation test suite
# Discover and execute the static analysis linter to verify that no untrusted input bypasses schema validation
```

**Accept when:**
- All schema validation test suites execute and pass with zero assertion failures across all schema modules.
- Static type checking completes with zero type errors across all derived schema types and parsing invocations.
- Boundary ingestion points reject malformed payloads with structured validation errors while accepting conformant data.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipelines enforce automated test execution and static type checks across all modules.
</enforcement>