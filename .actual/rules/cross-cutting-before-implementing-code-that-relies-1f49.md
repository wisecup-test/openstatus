---
version: 2
name: cross-cutting-before-implementing-code-that-relies-1f49
description: "Zod Schema Validation Library Adoption for Error Handling: Before Implementing Code That Relies Versioned"
metadata:
  actual:
    adr_id: 1f4954bb-d9fe-406c-9d9b-34c0bd0b138f
    status: accepted
    candidate_group_id: 43b62c44-b9f5-416e-a824-27861fa6976d
    evidence:
      source: deep
  scope:
    root: packages/error
    paths:
      - src/**
---

# Zod Schema Validation Library Adoption for Error Handling: Before Implementing Code That Relies Versioned

Scope: applies to `packages/error/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST: Before implementing code that relies on versioned dependencies, the consumer MUST inspect the workspace dependency manifests and resolution artifacts to determine the exact locked version of Zod.

### Verify

```bash
# Discover the project dependency manifest and verify that Zod is declared as a direct dependency for error management modules.
# Discover and execute the project test runner to ensure all error code validation and schema error tests pass.
# Discover and execute the workspace static type checker to confirm zero type discrepancies between Zod schemas and inferred error types.
```

**Accept when:**
- All verification test suites covering schema errors, error codes, and error utilities pass without failures.
- Static type verification passes with zero diagnostic errors across Zod schema definitions and their inferred TypeScript types.
- No extraneous or conflicting third-party schema validation libraries are introduced into the error handling module dependency tree.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>