---
version: 2
name: cross-cutting-domain-data-models-table-definitions-e69f
description: "Adoption of @openstatus/db Internal Package for Database Access and Schema Definitions: Domain Data Models Table Definitions Associated"
metadata:
  actual:
    adr_id: e69f66c7-4b62-4006-9b46-8556d3abf855
    status: accepted
    candidate_group_id: 6a59cfe8-1e88-4c01-871a-1db91c9d4ce9
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/**
      - src/lib/**
---

# Adoption of @openstatus/db Internal Package for Database Access and Schema Definitions: Domain Data Models Table Definitions Associated

Scope: applies to `apps/web/src/app/api/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for all application route handlers, scheduled tasks, and domain utility modules that query relational storage or reference database entity types.

### Rules

- **R-DB-001** MUST: Domain data models, table definitions, and associated schema constants MUST be declared in @openstatus/db/src/schema to preserve unified contract boundaries across consuming modules.
- **R-DB-002** MUST: Ensure newly introduced database table schemas and relational models are defined within the shared database module and exported through public module subpaths before importing them into endpoint handlers.
- **R-DB-003** MUST: Verify that all required environment configuration values for database client initialization are validated prior to executing database queries.

### Verify

```bash
# Discover and execute the project static analysis and type verification scripts to confirm database imports resolve through @openstatus/db
# Discover and run the repository test suite to verify database access routines pass without regression
```

**Accept when:**
- All database access and schema imports resolve through @openstatus/db without unresolved module references.
- Static type verification and automated test suites execute with zero errors across all consuming modules.

<enforcement>
Claude Code MUST NOT skip or defer verification. All database persistence logic and schema references must use the designated internal database package.
</enforcement>