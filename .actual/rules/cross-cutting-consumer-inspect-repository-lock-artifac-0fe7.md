---
version: 2
name: cross-cutting-consumer-inspect-repository-lock-artifac-0fe7
description: "Zod Schema Definition and Runtime Input Validation: Consumer Inspect Repository Lock Artifact Determine"
metadata:
  actual:
    adr_id: 0fe7d411-382f-41fd-9214-0bb640f0766f
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

# Zod Schema Definition and Runtime Input Validation: Consumer Inspect Repository Lock Artifact Determine

Scope: applies to `packages/db/env.mjs`, `packages/db/script/**`, `packages/db/src/schema/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST: The consumer MUST inspect the repository lock artifact to determine the exact resolved version of the schema validation library before introducing or modifying schema declarations.

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
Claude Code MUST NOT skip or defer verification. Verified by continuous integration pipelines, automated tests, static analysis, and peer code reviews.
</enforcement>