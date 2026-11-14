---
version: 2
name: cross-cutting-when-fetching-associated-entities-across-000a
description: "@openstatus/db In-Memory Entity Indexing and Relational Assembly: When Fetching Associated Entities Across Multiple"
metadata:
  actual:
    adr_id: 000aaa82-d505-43c1-afd1-e9fb4a759e89
    status: accepted
    candidate_group_id: 828efdcd-90f3-452e-9a8c-a21e70de4bee
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/**
---

# @openstatus/db In-Memory Entity Indexing and Relational Assembly: When Fetching Associated Entities Across Multiple

Scope: applies to `packages/services/src/**`.

These rules are ALWAYS ACTIVE for domain service operations executing within the services layer that query or mutate primary datastore entities, including batch operations, list endpoints, and transactional updates requiring correlation across related datastore records.

### Rules

- **R-DB-001** MUST: When fetching associated entities across multiple parents, handlers MUST execute batch select operations via @openstatus/db and index the results into an in-memory map keyed by entity identifier or foreign key.

### Verify

```bash
# Discover and run the project verification script declared in the workspace configuration to validate in-memory aggregation behavior
# Discover workspace static analysis and type check command and execute across all service modules
# Discover and execute lint verification task from the repository task runner
```

**Accept when:**
- All service unit and integration tests executing batch aggregation and map indexing pass without errors
- Static analysis and type checking pass across all service packages interacting with the datastore module
- No iterative datastore query patterns within loops are detected in service handler implementations

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipelines, peer code review, and static analysis checks enforce compliance, and violations must be revised before merge approval.
</enforcement>