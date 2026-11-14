---
version: 2
name: cross-cutting-engineers-not-bypass-runtime-schema-8689
description: "Zod Schema Definition and Runtime Input Validation: Engineers Not Bypass Runtime Schema Validation"
metadata:
  actual:
    adr_id: 86897db5-1f68-4fe5-b23a-b6f113a3fb76
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

# Zod Schema Definition and Runtime Input Validation: Engineers Not Bypass Runtime Schema Validation

Scope: applies to `packages/db/env.mjs`, `packages/db/script/**`, `packages/db/src/schema/**`.

These rules are ALWAYS ACTIVE for domain entity schema declarations, relational model validation definitions, workspace limit definitions, audit log payloads, session message validation, environment variable ingestion, and database migration script input parameters.

### Rules

- **R-ZOD-001** MUST_NOT: Engineers MUST NOT bypass runtime schema validation by casting unvalidated incoming data directly to domain TypeScript interfaces.

### Verify

```bash
# Discover and execute the repository type-check script to validate schema definitions and type inference across all database schema modules.
# Discover and execute the project test runner against the schema validation test suite to confirm all validation rules and parsing constraints pass.
# Discover and execute the static analysis linter to verify that no untrusted input bypasses schema validation.
```

**Accept when:**
- All schema validation test suites execute and pass with zero assertion failures across all schema modules.
- Static type checking completes with zero type errors across all derived schema types and parsing invocations.
- Boundary ingestion points reject malformed payloads with structured validation errors while accepting conformant data.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>