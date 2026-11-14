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

These rules are ALWAYS ACTIVE for service packages requiring database queries, schema models, or entity state persistence, as well as event dispatch and notification channel modules managing subscriber records and status updates.

### Rules

- **R-DB-001** MUST_NOT: Service packages MUST NOT define duplicate or standalone database schema representations outside of the shared @openstatus/db hierarchy.
- **R-DB-002** MANDATORY: The consumer MUST discover tool names, file names, commands, package managers, and versions from the repository.
- **R-DB-003** MANDATORY (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, execute in order: find dependency manifest, identify build tool, inspect repository lock/resolution artifact for exact version, look up official documentation for that exact version, and confirm every API/class/function exists in that version.
- **R-DB-004** MUST: Import database query clients and relational schemas strictly from the authorized exports of the shared database module.
- **R-DB-005** MUST: Structure relational queries with deterministic ordering and explicit relational inclusions to ensure idempotency across asynchronous event processing.

### Verify

```bash
# Discover and run repository static analysis, linting, type-checking, and test scripts
# Example discovery & verification flow:
# 1. Inspect package.json for lint, test, and typecheck scripts
# 2. Run package manager test/lint/typecheck commands
```

**Accept when:**
- Static analysis confirms zero unauthorized database driver imports outside of the designated shared database module.
- All automated tests for notification dispatch and channel handlers pass with complete type verification and schema conformity.
- Entity queries in dispatch routines execute with deterministic sorting and explicit filtering without runtime schema errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis, workspace type checking, and peer code review enforce compliance.
</enforcement>