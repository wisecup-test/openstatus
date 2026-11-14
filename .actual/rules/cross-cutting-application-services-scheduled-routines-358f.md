---
version: 2
name: cross-cutting-application-services-scheduled-routines-358f
description: "Adoption of @openstatus/db Internal Package for Database Access and Schema Definitions: Application Services Scheduled Routines Domain Utilities"
metadata:
  actual:
    adr_id: 358f2714-a36a-46b6-90c4-a4c3f6bd0621
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

# Adoption of @openstatus/db Internal Package for Database Access and Schema Definitions: Application Services Scheduled Routines Domain Utilities

Scope: applies to `apps/web/src/app/api/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for all application route handlers, scheduled tasks, and domain utility modules that query relational storage or reference database entity types.

### Rules

- **R-DB-001** MUST: Application services, scheduled routines, and domain utilities MUST access database query clients and entity schema definitions exclusively through the @openstatus/db shared workspace package and its designated module entrypoints.

### Verify

```bash
# Discover and execute the project static analysis and type verification scripts
# Discover and run the repository test suite
```

**Accept when:**
- All database access and schema imports resolve through @openstatus/db without unresolved module references.
- Static type verification and automated test suites execute with zero errors across all consuming modules.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>