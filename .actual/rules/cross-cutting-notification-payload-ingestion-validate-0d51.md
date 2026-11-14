---
version: 2
name: cross-cutting-notification-payload-ingestion-validate-0d51
description: "Adoption of @openstatus/db/src/schema for Centralized Database Schema Definitions: Notification Payload Ingestion Validate Input Data"
metadata:
  actual:
    adr_id: 0d51e8d1-5ac2-4acd-9637-fb63d01e9d98
    status: accepted
    candidate_group_id: ecbc9ccf-571a-40d2-96cf-eacf4af4f4d9
    evidence:
      source: deep
  scope:
    root: packages/notifications/email
    paths:
      - src/**
---

# Adoption of @openstatus/db/src/schema for Centralized Database Schema Definitions: Notification Payload Ingestion Validate Input Data

Scope: applies to `packages/notifications/email/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DB-001** MUST: Notification payload ingestion MUST validate input data against schema definitions derived from the centralized schema module using safe parsing methods.

### Verify

```bash
# Discover the workspace static analysis and type checking script from the repository manifest and execute it to verify all imports resolve with complete type safety.
# Discover and execute the repository linting suite to ensure that no duplicate local database entity interfaces exist across domain notification packages.
# Discover and execute the automated test runner across notification packages to validate that mock suites and production handlers conform to shared schema definitions.
```

**Accept when:**
- All domain notification and test mock packages resolve database entity types and constants directly from @openstatus/db/src/schema without type errors.
- Static analysis confirms zero redundant database schema or entity definitions in domain notification directories.
- All unit and integration tests for notification processing and mock generation pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration checks and mandatory architectural peer reviews.
</enforcement>