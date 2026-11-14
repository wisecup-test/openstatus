---
version: 2
name: cross-cutting-domain-notification-handlers-mock-fixtur-af16
description: "Adoption of @openstatus/db/src/schema for Centralized Database Schema Definitions: Domain Notification Handlers Mock Fixtures Not"
metadata:
  actual:
    adr_id: af16befa-3378-4780-b809-9c6e201e2f20
    status: accepted
    candidate_group_id: ecbc9ccf-571a-40d2-96cf-eacf4af4f4d9
    evidence:
      source: deep
  scope:
    root: packages/notifications/email
    paths:
      - src/**
---

# Adoption of @openstatus/db/src/schema for Centralized Database Schema Definitions: Domain Notification Handlers Mock Fixtures Not

Scope: applies to `packages/notifications/email/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DB-001** MUST_NOT: Domain notification handlers and mock fixtures MUST_NOT declare duplicate or independent database schema definitions locally.

### Verify

```bash
# Discover the workspace static analysis and type checking script from the repository manifest and execute it to verify all imports of @openstatus/db/src/schema resolve with complete type safety.
# Discover and execute the repository linting suite to ensure that no duplicate local database entity interfaces exist across domain notification packages.
# Discover and execute the automated test runner across notification packages to validate that mock suites and production handlers conform to shared schema definitions.
```

**Accept when:**
- All domain notification and test mock packages resolve database entity types and constants directly from @openstatus/db/src/schema without type errors.
- Static analysis confirms zero redundant database schema or entity definitions in domain notification directories.
- All unit and integration tests for notification processing and mock generation pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification. Compliance with centralized schema definitions is strictly enforced via automated continuous integration checks and mandatory architectural peer reviews.
</enforcement>