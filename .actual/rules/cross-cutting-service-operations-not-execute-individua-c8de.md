---
version: 2
name: cross-cutting-service-operations-not-execute-individua-c8de
description: "@openstatus/db In-Memory Entity Indexing and Relational Assembly: Service Operations Not Execute Individual Datastore"
metadata:
  actual:
    adr_id: c8ded3a9-7966-47c9-83ca-5511722d3901
    status: accepted
    candidate_group_id: 828efdcd-90f3-452e-9a8c-a21e70de4bee
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/**
---

# @openstatus/db In-Memory Entity Indexing and Relational Assembly: Service Operations Not Execute Individual Datastore

Scope: applies to `packages/services/src/**`.

These rules are ALWAYS ACTIVE for all backend service operations interacting with primary datastore tables through @openstatus/db to service composite domain requests.

### Rules

- **R-DB-001** MUST_NOT: Service operations MUST NOT execute individual datastore queries inside loops to look up associated entities for an active batch.

### Verify

```bash
# Discover and run the workspace service test suite to validate in-memory aggregation behavior
# Discover and execute workspace static analysis and type check across all service modules
# Discover and execute the lint verification task from the repository task runner
```

**Accept when:**
- All service unit and integration tests executing batch aggregation and map indexing pass without errors
- Static analysis and type checking pass across all service packages interacting with the datastore module
- No iterative datastore query patterns within loops are detected in service handler implementations

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipelines, peer code review, and static analysis checks enforce compliance.
</enforcement>