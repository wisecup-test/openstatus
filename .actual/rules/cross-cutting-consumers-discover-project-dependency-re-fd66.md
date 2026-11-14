---
version: 2
name: cross-cutting-consumers-discover-project-dependency-re-fd66
description: "Adoption of @openstatus/db Shared Workspace Module for Persistence and Schema Access: Consumers Discover Project Dependency Resolution Artifact"
metadata:
  actual:
    adr_id: fd66e517-0717-4831-a26c-55752762ffae
    status: accepted
    candidate_group_id: d92b2fc8-976d-4c73-9cf2-ca95584cdb7a
    evidence:
      source: deep
  scope:
    root: packages/subscriptions
    paths:
      - src/**
---

# Adoption of @openstatus/db Shared Workspace Module for Persistence and Schema Access: Consumers Discover Project Dependency Resolution Artifact

Scope: applies to `packages/subscriptions/src/**`.

These rules are ALWAYS ACTIVE for service packages requiring database queries, schema models, or entity state persistence, and event dispatch and notification channel modules managing subscriber records and status updates.

### Rules

- **R-DB-001** MUST: Consumers MUST discover the project dependency resolution artifact and verify the exact resolved version of internal and external dependencies prior to implementation.
- **R-DB-002** MUST: Import database query clients and relational schemas strictly from the authorized exports of the shared database module.
- **R-DB-003** MUST: Structure relational queries with deterministic ordering and explicit relational inclusions to ensure idempotency across asynchronous event processing.

### Verify

```bash
# Discover and run repository static analysis, linting, type-checking, and tests to verify database dependency and query compliance
npm run lint || pnpm lint || yarn lint
npm run typecheck || pnpm typecheck || yarn typecheck
npm test || pnpm test || yarn test
```

**Accept when:**
- Static analysis confirms zero unauthorized database driver imports outside of the designated shared database module.
- All automated tests for notification dispatch and channel handlers pass with complete type verification and schema conformity.
- Entity queries in dispatch routines execute with deterministic sorting and explicit filtering without runtime schema errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>