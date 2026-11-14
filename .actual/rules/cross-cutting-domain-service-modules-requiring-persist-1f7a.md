---
version: 2
name: cross-cutting-domain-service-modules-requiring-persist-1f7a
description: "Adoption of @openstatus/db Shared Workspace Module for Persistence and Schema Access: Domain Service Modules Requiring Persistence Access"
metadata:
  actual:
    adr_id: 1f7a35bd-0a2b-49b6-af4e-dea166137905
    status: accepted
    candidate_group_id: d92b2fc8-976d-4c73-9cf2-ca95584cdb7a
    evidence:
      source: deep
  scope:
    root: packages/subscriptions
    paths:
      - src/**
---

# Adoption of @openstatus/db Shared Workspace Module for Persistence and Schema Access: Domain Service Modules Requiring Persistence Access

Scope: applies to `packages/subscriptions/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DB-001** MUST: Domain service modules requiring persistence access MUST consume database clients and schema definitions exclusively from the @openstatus/db shared workspace module.

### Verify

```bash
# Discover and run the repository static analysis and linting scripts to verify persistence imports
npm run lint
# Execute the project type-checking script
npm run typecheck
# Discover and execute the automated test suite
npm run test
```

**Accept when:**
- Static analysis confirms zero unauthorized database driver imports outside of the designated shared database module.
- All automated tests for notification dispatch and channel handlers pass with complete type verification and schema conformity.
- Entity queries in dispatch routines execute with deterministic sorting and explicit filtering without runtime schema errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis, type checking, and unit testing must pass without violations.
</enforcement>