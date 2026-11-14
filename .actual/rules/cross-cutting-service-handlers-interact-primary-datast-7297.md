---
version: 2
name: cross-cutting-service-handlers-interact-primary-datast-7297
description: "@openstatus/db In-Memory Entity Indexing and Relational Assembly: Service Handlers Interact Primary Datastore Entities"
metadata:
  actual:
    adr_id: 7297306b-66f7-41a5-a8c7-78c8ab6db0cd
    status: accepted
    candidate_group_id: 828efdcd-90f3-452e-9a8c-a21e70de4bee
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/**
---

# @openstatus/db In-Memory Entity Indexing and Relational Assembly: Service Handlers Interact Primary Datastore Entities

Scope: applies to `packages/services/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DS-001** MUST: Service handlers MUST interact with primary datastore entities exclusively through the @openstatus/db client and schema definitions, structuring multi-entity correlations using local in-memory index maps populated from batch queries.

### Verify

```bash
# Discover and run the project verification script declared in the workspace configuration
# Discover and execute workspace static analysis and type check command
# Discover and execute lint verification task from the repository task runner
```

**Accept when:**
- All service unit and integration tests executing batch aggregation and map indexing pass without errors
- Static analysis and type checking pass across all service packages interacting with the datastore module
- No iterative datastore query patterns within loops are detected in service handler implementations

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipelines, peer code review, and static analysis enforce these rules.
</enforcement>