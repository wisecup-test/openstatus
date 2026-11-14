---
version: 2
name: cross-cutting-raw-query-results-obtained-through-0de8
description: "Zod Schema Validation for Database Query Result Hydration: Raw Query Results Obtained Through Database"
metadata:
  actual:
    adr_id: 0de8f6df-326c-4978-bba6-d48d3560f354
    status: accepted
    candidate_group_id: a93d3679-6c55-490f-a8b5-255f0e5c5d25
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - script/**
---

# Zod Schema Validation for Database Query Result Hydration: Raw Query Results Obtained Through Database

Scope: applies to `packages/db/script/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST: Raw query results obtained through database select calls MUST be parsed with z.array or corresponding schema parse methods rather than cast with unchecked type assertions.

### Verify

```bash
# Discover and run the project static type checker to verify that all database query results pass through schema parse methods before mutation.
npx tsc --noEmit

# Discover and run the project test suite to validate that data access and migration routines reject invalid record shapes during schema parsing.
npm test
```

**Accept when:**
- All database query results in migration scripts are parsed through Zod schemas prior to data transformation.
- The project type verification and test suites pass without schema parsing or type errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>