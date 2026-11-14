---
version: 2
name: cross-cutting-domain-notification-modules-test-mock-5ca1
description: "Adoption of @openstatus/db/src/schema for Centralized Database Schema Definitions: Domain Notification Modules Test Mock Suites"
metadata:
  actual:
    adr_id: 5ca1036d-d7f8-44a6-a1d5-f19e94372d61
    status: accepted
    candidate_group_id: ecbc9ccf-571a-40d2-96cf-eacf4af4f4d9
    evidence:
      source: deep
  scope:
    root: packages/notifications/email
    paths:
      - src/**
---

# Adoption of @openstatus/db/src/schema for Centralized Database Schema Definitions: Domain Notification Modules Test Mock Suites

Scope: applies to `packages/notifications/email/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SCH-001** MUST: Domain notification modules and test mock suites MUST import database entity schemas and schema constants exclusively from the internal @openstatus/db/src/schema module.

### Verify

```bash
# Discover the workspace static analysis and type checking script from the repository manifest and execute it
npm run typecheck

# Discover and execute the repository linting suite
npm run lint

# Discover and execute the automated test runner across notification packages
npm test
```

**Accept when:**
- All domain notification and test mock packages resolve database entity types and constants directly from @openstatus/db/src/schema without type errors.
- Static analysis confirms zero redundant database schema or entity definitions in domain notification directories.
- All unit and integration tests for notification processing and mock generation pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>