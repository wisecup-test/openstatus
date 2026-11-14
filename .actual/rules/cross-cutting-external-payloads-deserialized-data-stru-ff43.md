---
version: 2
name: cross-cutting-external-payloads-deserialized-data-stru-ff43
description: "Zod Schema Definition and Runtime Input Validation: External Payloads Deserialized Data Structures Environment"
metadata:
  actual:
    adr_id: ff438880-c729-4258-9d93-d1c6924025f5
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

# Zod Schema Definition and Runtime Input Validation: External Payloads Deserialized Data Structures Environment

Scope: applies to `packages/db/env.mjs`, `packages/db/script/**`, `packages/db/src/schema/**`.

These rules are ALWAYS ACTIVE for all external payloads, deserialized data structures, workspace limit overrides, relational migration scripts, and runtime environment inputs entering persistence tiers or internal service layers.

### Rules

- **R-ZOD-001** MUST: All external payloads, deserialized data structures, and environment inputs MUST be validated through schema parsing methods prior to reaching database transactions or internal service layers.

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
Claude Code MUST NOT skip or defer verification. Verification is mandatory via continuous integration pipelines, peer code reviews, and static analysis checks enforcing automated test execution and schema validation across all modules.
</enforcement>