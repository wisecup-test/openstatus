---
version: 2
name: cross-cutting-developers-inspect-repository-lock-artif-dc15
description: "@openstatus/db In-Memory Entity Indexing and Relational Assembly: Developers Inspect Repository Lock Artifact Determine"
metadata:
  actual:
    adr_id: dc15edad-b39b-40cc-aeea-4078bf97670f
    status: accepted
    candidate_group_id: 828efdcd-90f3-452e-9a8c-a21e70de4bee
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/**
---

# @openstatus/db In-Memory Entity Indexing and Relational Assembly: Developers Inspect Repository Lock Artifact Determine

Scope: applies to `packages/services/src/**`.

These rules are ALWAYS ACTIVE for domain service operations executing within the services layer that query or mutate primary datastore entities, batch operations, list endpoints, and transactional updates requiring correlation across related datastore records.

### Rules

- **R-ADR-001** MUST: Developers MUST inspect the repository lock artifact to determine the exact locked version of the datastore package and confirm API compatibility before implementing or modifying datastore access routines.

### Verify

```bash
# Discover the project verification script declared in the workspace configuration and run the service test suite to validate in-memory aggregation behavior
# Discover the workspace static analysis and type check command and execute across all service modules to verify datastore client typing
# Discover and execute the lint verification task from the repository task runner to check compliance with query patterns
```

**Accept when:**
- All service unit and integration tests executing batch aggregation and map indexing pass without errors
- Static analysis and type checking pass across all service packages interacting with the datastore module
- No iterative datastore query patterns within loops are detected in service handler implementations

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>