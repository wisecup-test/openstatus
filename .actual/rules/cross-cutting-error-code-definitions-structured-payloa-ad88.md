---
version: 2
name: cross-cutting-error-code-definitions-structured-payloa-ad88
description: "Zod Schema Validation Library Adoption for Error Handling: Error Code Definitions Structured Payloads Validated"
metadata:
  actual:
    adr_id: ad888436-ce2c-4a58-b62b-a2ee84e20600
    status: accepted
    candidate_group_id: 43b62c44-b9f5-416e-a824-27861fa6976d
    evidence:
      source: deep
  scope:
    root: packages/error
    paths:
      - src/**
---

# Zod Schema Validation Library Adoption for Error Handling: Error Code Definitions Structured Payloads Validated

Scope: applies to `packages/error/src/**`.

These rules are ALWAYS ACTIVE for error code registries, schema-validated error classes, and error parsing utilities across error handling packages, as well as runtime validation of structured error payloads and diagnostic context objects.

### Rules

- **R-ZOD-001** MUST: Error code definitions and structured error payloads MUST be validated through Zod schema declarations rather than unstructured primitive types.

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