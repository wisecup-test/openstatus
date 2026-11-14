---
version: 2
name: cross-cutting-engineers-verify-exact-resolved-version-b0c3
description: "Zod Schema Validation for Database Query Result Hydration: Engineers Verify Exact Resolved Version Zod"
metadata:
  actual:
    adr_id: b0c31a8e-9d5a-4e06-87ea-3fcaab320d65
    status: accepted
    candidate_group_id: a93d3679-6c55-490f-a8b5-255f0e5c5d25
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - script/**
---

# Zod Schema Validation for Database Query Result Hydration: Engineers Verify Exact Resolved Version Zod

Scope: applies to `packages/db/script/**`.

These rules are ALWAYS ACTIVE for database migration scripts and data access routines that retrieve and transform persisted records.

### Rules

- **R-ZOD-001** MUST: Engineers MUST verify the exact resolved version of Zod from the repository lock artifact and confirm API compatibility before implementing or modifying schema validation routines.

### Verify

```bash
# Discover and run the project static type checker to verify that all database query results pass through schema parse methods before mutation.
# Discover and run the project test suite to validate that data access and migration routines reject invalid record shapes during schema parsing.
```

**Accept when:**
- All database query results in migration scripts are parsed through Zod schemas prior to data transformation.
- The project type verification and test suites pass without schema parsing or type errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>