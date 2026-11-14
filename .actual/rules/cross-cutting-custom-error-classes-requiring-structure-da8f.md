---
version: 2
name: cross-cutting-custom-error-classes-requiring-structure-da8f
description: "Zod Schema Validation Library Adoption for Error Handling: Custom Error Classes Requiring Structured Diagnostic"
metadata:
  actual:
    adr_id: da8feef4-d98c-406c-9324-68458f6318c9
    status: accepted
    candidate_group_id: 43b62c44-b9f5-416e-a824-27861fa6976d
    evidence:
      source: deep
  scope:
    root: packages/error
    paths:
      - src/**
---

# Zod Schema Validation Library Adoption for Error Handling: Custom Error Classes Requiring Structured Diagnostic

Scope: applies to `packages/error/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST: Custom error classes requiring structured diagnostic context MUST derive static TypeScript types directly from Zod schemas via schema type inference.

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