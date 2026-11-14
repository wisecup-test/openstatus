---
version: 2
name: cross-cutting-handlers-maintain-composite-string-keys-736d
description: "@openstatus/db In-Memory Entity Indexing and Relational Assembly: Handlers Maintain Composite String Keys Index"
metadata:
  actual:
    adr_id: 736d5398-1396-4cf8-9ec7-b076cd4fe8b8
    status: accepted
    candidate_group_id: 828efdcd-90f3-452e-9a8c-a21e70de4bee
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/**
---

# @openstatus/db In-Memory Entity Indexing and Relational Assembly: Handlers Maintain Composite String Keys Index

Scope: applies to `packages/services/src/**`.

These rules are ALWAYS ACTIVE for domain service operations executing within the services layer that query or mutate primary datastore entities, batch operations, list endpoints, and transactional updates requiring correlation across related datastore records.

### Rules

- **R-OPN-001** MAY: Handlers MAY maintain composite string keys in the index map when correlating entities across multi-column identifiers or partitioned time windows.

### Verify

```bash
# Discover and run the project verification script declared in workspace configuration
# Run the service test suite to validate in-memory aggregation behavior
# Discover and execute workspace static analysis and type check command across all service modules
# Discover and execute the lint verification task from the repository task runner
```

**Accept when:**
- All service unit and integration tests executing batch aggregation and map indexing pass without errors
- Static analysis and type checking pass across all service packages interacting with the datastore module
- No iterative datastore query patterns within loops are detected in service handler implementations

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests exhibiting iterative datastore queries inside loops or missing index structures must be revised before merge approval.
</enforcement>