---
version: 2
name: cross-cutting-domain-service-modules-not-import-e28f
description: "Adoption of @openstatus/db/src/schema for Centralized Database Schema Definitions: Domain Service Modules Not Import Internal"
metadata:
  actual:
    adr_id: e28f24dd-4516-4656-8d6e-7108aed49a78
    status: accepted
    candidate_group_id: ecbc9ccf-571a-40d2-96cf-eacf4af4f4d9
    evidence:
      source: deep
  scope:
    root: packages/notifications/email
    paths:
      - src/**
---

# Adoption of @openstatus/db/src/schema for Centralized Database Schema Definitions: Domain Service Modules Not Import Internal

Scope: applies to `packages/notifications/email/src/**`.

These rules are ALWAYS ACTIVE for notification service packages handling database-backed event processing and test mock generation suites creating mock database entity representations.

### Rules

- **R-SCHEMA-001** SHOULD_NOT: Domain service modules SHOULD_NOT import internal database connection or migration drivers directly when schema definitions alone satisfy typing and validation requirements.
- **R-SCHEMA-002** MANDATORY: Import schema objects and constants directly from the designated schema entrypoint of the internal database package.
- **R-SCHEMA-003** MANDATORY: Use safe schema parsing functions to validate incoming notification payloads against imported schema definitions prior to executing business logic.
- **R-SCHEMA-004** MANDATORY: Execute in order lock-version grounding steps: find dependency manifest, identify build tool, inspect lock or resolution artifact for exact version, look up official documentation, confirm API existence, and re-run per dependency at point of use before writing code that uses a versioned library.

### Verify

```bash
# Discover and execute the workspace static analysis and type checking script from the repository manifest
# Example: pnpm check / npm run typecheck

# Discover and execute the repository linting suite to ensure no duplicate local database entity interfaces exist
# Example: pnpm lint / npm run lint

# Discover and execute the automated test runner across notification packages
# Example: pnpm test / npm test
```

**Accept when:**
- All domain notification and test mock packages resolve database entity types and constants directly from @openstatus/db/src/schema without type errors.
- Static analysis confirms zero redundant database schema or entity definitions in domain notification directories.
- All unit and integration tests for notification processing and mock generation pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated continuous integration checks and mandatory architectural peer review.
</enforcement>