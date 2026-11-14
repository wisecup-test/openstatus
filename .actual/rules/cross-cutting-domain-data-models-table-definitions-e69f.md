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

- **R-DB-001** MUST: Declare domain data models, table definitions, and associated schema constants in @openstatus/db/src/schema to preserve unified contract boundaries across consuming modules.

### Verify

```bash
# Discover and execute the project static analysis and type verification scripts
# Discover and run the repository test suite
```

**Accept when:**
- All database access and schema imports resolve through @openstatus/db without unresolved module references.
- Static type verification and automated test suites execute with zero errors across all consuming modules.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration pipelines and peer code review.
</enforcement>