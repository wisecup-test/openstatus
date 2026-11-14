---
version: 2
name: cross-cutting-test-mock-suites-construct-database-e5fd
description: "Adoption of @openstatus/db/src/schema for Centralized Database Schema Definitions: Test Mock Suites Construct Database Entity"
metadata:
  actual:
    adr_id: e5fd4467-0c20-45f2-99d5-9a96789ca48e
    status: accepted
    candidate_group_id: ecbc9ccf-571a-40d2-96cf-eacf4af4f4d9
    evidence:
      source: deep
  scope:
    root: packages/notifications/email
    paths:
      - src/**
---

# Adoption of @openstatus/db/src/schema for Centralized Database Schema Definitions: Test Mock Suites Construct Database Entity

Scope: applies to `packages/notifications/email/src/**`.

These rules are ALWAYS ACTIVE for notification service packages handling database-backed event processing and test mock generation suites creating mock database entity representations.

### Rules

- **R-DB-001** SHOULD: Test mock suites SHOULD construct database entity fixtures by composing typed schema models provided by @openstatus/db/src/schema to guarantee compatibility with production schema contracts.

### Verify

```bash
# Discover and run the workspace static analysis and type checking script from the repository manifest
# Discover and run the repository linting suite
# Discover and run the automated test runner across notification packages
```

**Accept when:**
- All domain notification and test mock packages resolve database entity types and constants directly from @openstatus/db/src/schema without type errors.
- Static analysis confirms zero redundant database schema or entity definitions in domain notification directories.
- All unit and integration tests for notification processing and mock generation pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification. All domain notification and test mock packages must resolve database entity types and constants directly from the central schema module, verified by automated continuous integration checks and architectural peer review.
</enforcement>