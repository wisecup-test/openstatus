---
version: 2
name: cross-cutting-data-ingestion-providers-mapper-componen-6d20
description: "In-Memory Key-Value Mapping Pattern in Importer Providers: Data Ingestion Providers Mapper Components Perform"
metadata:
  actual:
    adr_id: 6d20256c-d613-4ec3-b82f-0a01dcdf0eb0
    status: accepted
    candidate_group_id: b7728fd2-3ad8-4ede-ba50-a11f197b9682
    evidence:
      source: deep
  scope:
    root: packages/importers
    paths:
      - src/providers/**
---

# In-Memory Key-Value Mapping Pattern in Importer Providers: Data Ingestion Providers Mapper Components Perform

Scope: applies to `packages/importers/src/providers/**`.

These rules are ALWAYS ACTIVE for entity mapping routines within data ingestion providers requiring identifier resolution across related data structures.

### Rules

- **R-IM-001** MUST: Data ingestion providers and mapper components MUST perform relational identifier resolution using transient in-memory lookup mappings scoped to the execution context rather than querying external datastores.
- **R-IM-002** MUST: Ensure that lookup structures are instantiated per import execution and dereferenced upon mapper completion to enable garbage collection.
- **R-IM-003** MUST: Normalize identifier keys to consistent string representations prior to insertion and retrieval to prevent type-mismatch misses.

### Verify

```bash
# Discover and execute the project unit and integration test suites that validate provider mapping routines and identifier resolution behavior.
# Discover and run the static analysis and linting scripts to verify absence of undeclared datastore dependencies within provider modules.
```

**Accept when:**
- All provider mapping test suites pass with complete resolution of related entity identifiers.
- Static analysis confirms no persistent datastore connections or unauthorized external cache dependencies are imported in provider mapping modules.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>