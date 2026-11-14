---
version: 2
name: cross-cutting-service-handlers-clear-discard-memory-b6a5
description: "@openstatus/db In-Memory Entity Indexing and Relational Assembly: Service Handlers Clear Discard Memory Index"
metadata:
  actual:
    adr_id: b6a55520-57c3-4eb9-b1b6-5156d1f55eab
    status: accepted
    candidate_group_id: 828efdcd-90f3-452e-9a8c-a21e70de4bee
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/**
---

# @openstatus/db In-Memory Entity Indexing and Relational Assembly: Service Handlers Clear Discard Memory Index

Scope: applies to `packages/services/src/**`.

These rules are ALWAYS ACTIVE for domain service operations executing within the services layer that query or mutate primary datastore entities, batch operations, list endpoints, and transactional updates requiring correlation across related datastore records.

### Rules

- **R-DB-001** SHOULD: Service handlers SHOULD clear or discard in-memory index maps at the termination of the request or transaction lifecycle to avoid unbounded memory retention.

### Verify

```bash
# Discover and run the project verification script declared in workspace configuration for the service test suite
# Discover and execute workspace static analysis and type check command
# Discover and execute lint verification task from repository task runner
```

**Accept when:**
- All service unit and integration tests executing batch aggregation and map indexing pass without errors
- Static analysis and type checking pass across all service packages interacting with the datastore module
- No iterative datastore query patterns within loops are detected in service handler implementations

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipelines, peer code review, and static analysis checks enforce that datastore queries avoid query iteration in loops, properly structure in-memory indexes, and execute schema parsing on datastore results.
</enforcement>