---
version: 2
name: cross-cutting-service-packages-not-define-duplicate-76cd
description: "Adoption of @openstatus/db Shared Workspace Module for Persistence and Schema Access: Service Packages Not Define Duplicate Standalone"
metadata:
  actual:
    adr_id: 76cdea6b-0e19-4e81-b9ee-2fc2e138b6db
    status: accepted
    candidate_group_id: d92b2fc8-976d-4c73-9cf2-ca95584cdb7a
    evidence:
      source: deep
  scope:
    root: packages/subscriptions
    paths:
      - src/**
---

# Adoption of @openstatus/db Shared Workspace Module for Persistence and Schema Access: Service Packages Not Define Duplicate Standalone

Scope: applies to `packages/subscriptions/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DB-001** MUST_NOT: Service packages MUST NOT define duplicate or standalone database schema representations outside of the shared @openstatus/db hierarchy.

### Verify

```bash
# Discover and run the repository static analysis, linting, and type-checking scripts
# to verify that persistence imports originate exclusively from the authorized shared database module.
# (Run the project's package manager script for linting/type-checking, e.g., npm run lint / npm run typecheck)
```

**Accept when:**
- Static analysis confirms zero unauthorized database driver imports outside of the designated shared database module.
- All automated tests for notification dispatch and channel handlers pass with complete type verification and schema conformity.
- Entity queries in dispatch routines execute with deterministic sorting and explicit filtering without runtime schema errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis, linting, and type-checking in the continuous integration pipeline are mandatory for all persistence queries and schema modifications.
</enforcement>