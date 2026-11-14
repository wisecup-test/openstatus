---
version: 2
name: cross-cutting-memory-map-entries-parsed-validated-3f7b
description: "@openstatus/db In-Memory Entity Indexing and Relational Assembly: Memory Map Entries Parsed Validated Against"
metadata:
  actual:
    adr_id: 3f7bb60d-ad03-4b59-b37c-bf5e37a8cd98
    status: accepted
    candidate_group_id: 828efdcd-90f3-452e-9a8c-a21e70de4bee
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/**
---

# @openstatus/db In-Memory Entity Indexing and Relational Assembly: Memory Map Entries Parsed Validated Against

Scope: applies to `packages/services/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DB-001** SHOULD: In-memory map entries SHOULD be parsed and validated against domain schemas immediately upon retrieval from @openstatus/db before insertion into the index structure.

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