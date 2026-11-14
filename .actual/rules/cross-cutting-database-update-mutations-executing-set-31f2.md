---
version: 2
name: cross-cutting-database-update-mutations-executing-set-31f2
description: "Zod Schema Validation for Database Query Result Hydration: Database Update Mutations Executing Set Operations"
metadata:
  actual:
    adr_id: 31f24e06-1f48-4d8e-a431-062e50190952
    status: accepted
    candidate_group_id: a93d3679-6c55-490f-a8b5-255f0e5c5d25
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - script/**
---

# Zod Schema Validation for Database Query Result Hydration: Database Update Mutations Executing Set Operations

Scope: applies to `packages/db/script/**`.

These rules are ALWAYS ACTIVE for database migration scripts and data access routines that retrieve and transform persisted records.

### Rules

- **R-ZOD-001** MUST_NOT: Database update mutations executing set operations on migrated records MUST NOT proceed if schema parse validation fails or raises validation exceptions.

### Verify

```bash
# Discover and run the project static type checker to verify that all database query results pass through schema parse methods before mutation.
# Discover and run the project test suite to validate that data access and migration routines reject invalid record shapes during schema parsing.
```

**Accept when:**
- All database query results in migration scripts are parsed through Zod schemas prior to data transformation.
- The project type verification and test suites pass without schema parsing or type errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. All database update mutations must be verified against schema validation requirements.
</enforcement>