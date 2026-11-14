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

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DB-001** MUST_NOT: Service operations MUST NOT execute individual datastore queries inside loops to look up associated entities for an active batch.

### Verify

```bash
# Discover the project verification script declared in the workspace configuration and run the service test suite
# Discover the workspace static analysis and type check command and execute across all service modules
# Discover and execute the lint verification task from the repository task runner
```

**Accept when:**
- All service unit and integration tests executing batch aggregation and map indexing pass without errors
- Static analysis and type checking pass across all service packages interacting with the datastore module
- No iterative datastore query patterns within loops are detected in service handler implementations

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>