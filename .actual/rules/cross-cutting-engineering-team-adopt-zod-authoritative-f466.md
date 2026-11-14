---
version: 2
name: cross-cutting-engineering-team-adopt-zod-authoritative-f466
description: "Zod Schema Definition and Runtime Input Validation: Engineering Team Adopt Zod Authoritative Schema"
metadata:
  actual:
    adr_id: f466eb16-704e-40f4-b5c1-a503b36d282b
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

# Zod Schema Definition and Runtime Input Validation: Engineering Team Adopt Zod Authoritative Schema

Scope: applies to `packages/db/env.mjs`, `packages/db/script/**`, `packages/db/src/schema/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST: The engineering team MUST adopt Zod as the authoritative schema declaration and runtime input validation library for all data layer entities, configuration inputs, and persistence boundaries.

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