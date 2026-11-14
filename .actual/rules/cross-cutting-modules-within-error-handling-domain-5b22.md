---
version: 2
name: cross-cutting-modules-within-error-handling-domain-5b22
description: "Zod Schema Validation Library Adoption for Error Handling: Modules Within Error Handling Domain Not"
metadata:
  actual:
    adr_id: 5b224a84-93a4-49c3-b638-6708265e3215
    status: accepted
    candidate_group_id: 43b62c44-b9f5-416e-a824-27861fa6976d
    evidence:
      source: deep
  scope:
    root: packages/error
    paths:
      - src/**
---

# Zod Schema Validation Library Adoption for Error Handling: Modules Within Error Handling Domain Not

Scope: applies to `packages/error/src/**`.

These rules are ALWAYS ACTIVE for all error code registries, schema-validated error classes, and error parsing utilities across error handling packages.

### Rules

- **R-ZOD-001** MUST_NOT: Modules within the error handling domain MUST NOT introduce competing third-party schema validation libraries.

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
Claude Code MUST NOT skip or defer verification. Automated CI pipelines and code reviews enforce that error definitions use Zod schemas and do not introduce unapproved validation libraries.
</enforcement>