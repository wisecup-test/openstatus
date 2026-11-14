---
version: 2
name: cross-cutting-data-access-migration-operations-that-3b4d
description: "Zod Schema Validation for Database Query Result Hydration: Data Access Migration Operations That Retrieve"
metadata:
  actual:
    adr_id: 3b4dbb64-81e0-4950-8ad1-cc88f3aca7f4
    status: accepted
    candidate_group_id: a93d3679-6c55-490f-a8b5-255f0e5c5d25
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - script/**
---

# Zod Schema Validation for Database Query Result Hydration: Data Access Migration Operations That Retrieve

Scope: applies to `packages/db/script/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST: Data access migration operations that retrieve raw records from database tables MUST validate query result sets using Zod schema parsing before applying data transformations or update mutations.

### Verify

```bash
# Discover and run the project static type checker to verify database query results
# Discover and run the project test suite to validate data access and migration routines
```

**Accept when:**
- All database query results in migration scripts are parsed through Zod schemas prior to data transformation.
- The project type verification and test suites pass without schema parsing or type errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. All database query results must pass through schema parse methods before mutation.
</enforcement>