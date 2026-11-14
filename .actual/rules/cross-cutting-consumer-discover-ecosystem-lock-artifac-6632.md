---
version: 2
name: cross-cutting-consumer-discover-ecosystem-lock-artifac-6632
description: "Adoption of @openstatus/db/src/schema for Centralized Database Schema Definitions: Consumer Discover Ecosystem Lock Artifact Repository"
metadata:
  actual:
    adr_id: 6632f9bc-4a26-4cda-8e2e-cc11eb045a3f
    status: accepted
    candidate_group_id: ecbc9ccf-571a-40d2-96cf-eacf4af4f4d9
    evidence:
      source: deep
  scope:
    root: packages/notifications/email
    paths:
      - src/**
---

# Adoption of @openstatus/db/src/schema for Centralized Database Schema Definitions: Consumer Discover Ecosystem Lock Artifact Repository

Scope: applies to `packages/notifications/email/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ADR-001** MUST: The consumer MUST discover the ecosystem lock artifact in the repository and verify the exact resolved version of all internal workspace dependencies before writing code.

### Verify

```bash
# Discover and execute the workspace static analysis and type checking script from the repository manifest
# Discover and execute the repository linting suite
# Discover and execute the automated test runner across notification packages
```

**Accept when:**
- All domain notification and test mock packages resolve database entity types and constants directly from @openstatus/db/src/schema without type errors.
- Static analysis confirms zero redundant database schema or entity definitions in domain notification directories.
- All unit and integration tests for notification processing and mock generation pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>