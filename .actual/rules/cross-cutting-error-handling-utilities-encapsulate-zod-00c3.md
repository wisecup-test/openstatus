---
version: 2
name: cross-cutting-error-handling-utilities-encapsulate-zod-00c3
description: "Zod Schema Validation Library Adoption for Error Handling: Error Handling Utilities Encapsulate Zod Parsing"
metadata:
  actual:
    adr_id: 00c36bd0-1b09-42fa-bd48-21fa70fee224
    status: accepted
    candidate_group_id: 43b62c44-b9f5-416e-a824-27861fa6976d
    evidence:
      source: deep
  scope:
    root: packages/error
    paths:
      - src/**
---

# Zod Schema Validation Library Adoption for Error Handling: Error Handling Utilities Encapsulate Zod Parsing

Scope: applies to `packages/error/src/**`.

These rules are ALWAYS ACTIVE for error code registries, schema-validated error classes, and error parsing utilities across error handling packages.

### Rules

- **R-ZOD-001** SHOULD: Error handling utilities SHOULD encapsulate Zod parsing methods and handle schema parsing failures by producing standardized diagnostic logging.
- **R-ZOD-002** MANDATORY: Discover the project dependency manifest and verify that Zod is declared as a direct dependency for error management modules.
- **R-ZOD-003** MANDATORY: Execute the project dependency build tool, lock resolution checks, or test runner to ensure all error code validation and schema error tests pass.
- **R-ZOD-004** MANDATORY: Execute the workspace static type checker to confirm zero type discrepancies between Zod schemas and inferred error types.

### Verify

```bash
# Discover the project dependency manifest and verify that Zod is declared as a direct dependency
grep -q '"zod"' package.json || grep -q 'zod' Cargo.toml || find . -name 'pom.xml' -o -name 'build.gradle*' | xargs grep -q 'zod'

# Discover and execute test suites covering schema errors, error codes, and error utilities
# (Repository-specific test runner command should be used, e.g., npm test / cargo test)

# Discover and execute workspace static type checking
# (Repository-specific type check command should be used, e.g., npx tsc --noEmit)
```

**Accept when:**
- All verification test suites covering schema errors, error codes, and error utilities pass without failures.
- Static type verification passes with zero diagnostic errors across Zod schema definitions and their inferred TypeScript types.
- No extraneous or conflicting third-party schema validation libraries are introduced into the error handling module dependency tree.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration pipelines executing workspace unit tests, static type checking, and dependency governance scans auditing workspace manifests.
</enforcement>