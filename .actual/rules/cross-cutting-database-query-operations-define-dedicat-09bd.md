---
version: 2
name: cross-cutting-database-query-operations-define-dedicat-09bd
description: "Zod Schema Validation for Database Query Result Hydration: Database Query Operations Define Dedicated Selection"
metadata:
  actual:
    adr_id: 09bd679e-5687-4e08-943f-ee818a81b1f6
    status: accepted
    candidate_group_id: a93d3679-6c55-490f-a8b5-255f0e5c5d25
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - script/**
---

# Zod Schema Validation for Database Query Result Hydration: Database Query Operations Define Dedicated Selection

Scope: applies to `packages/db/script/**`.

These rules are ALWAYS ACTIVE for database migration scripts and data access routines that retrieve and transform persisted records.

### Rules

- **R-ZOD-001** SHOULD: Database query operations SHOULD define dedicated selection schemas alongside table definitions to govern query result validation across data access routines.

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