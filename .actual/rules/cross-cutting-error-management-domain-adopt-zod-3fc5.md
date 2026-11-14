---
version: 2
name: cross-cutting-error-management-domain-adopt-zod-3fc5
description: "Zod Schema Validation Library Adoption for Error Handling: Error Management Domain Adopt Zod Authoritative"
metadata:
  actual:
    adr_id: 3fc5df8c-82d3-4380-9373-fb02ae09920f
    status: accepted
    candidate_group_id: 43b62c44-b9f5-416e-a824-27861fa6976d
    evidence:
      source: deep
  scope:
    root: packages/error
    paths:
      - src/**
---

# Zod Schema Validation Library Adoption for Error Handling: Error Management Domain Adopt Zod Authoritative

Scope: applies to `packages/error/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ERR-001** MUST: The error management domain MUST adopt Zod as the authoritative schema declaration and runtime validation library for all error code definitions, schema error classes, and structured error payloads.

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
Claude Code MUST NOT skip or defer verification. Automated continuous integration pipelines execute workspace unit tests, static type checking, and dependency governance audits.
</enforcement>