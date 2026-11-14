---
version: 2
name: cross-cutting-intermediate-resource-indexing-caching-m-9d2a
description: "Importer Provider Client and Mapper Submodule Architecture: Intermediate Resource Indexing Caching Mechanisms Used"
metadata:
  actual:
    adr_id: 9d2a5725-8041-4846-8abd-5429d633855a
    status: accepted
    candidate_group_id: 1c1bfc9d-9f02-4f19-ba60-a13c7cefdafb
    evidence:
      source: deep
  scope:
    root: packages/importers
    paths:
      - src/providers/**
---

# Importer Provider Client and Mapper Submodule Architecture: Intermediate Resource Indexing Caching Mechanisms Used

Scope: applies to `packages/importers/src/providers/**`.

These rules are ALWAYS ACTIVE for all external service integration providers within the data importer subsystem and module implementations handling external API ingestion, normalization, and state indexing.

### Rules

- **R-IMP-001** SHOULD: Intermediate resource indexing and caching mechanisms used during ingestion SHOULD be encapsulated within the provider orchestration or mapper boundary without exposing internal cache handles to external callers.

### Verify

```bash
# Discover and execute the repository static analysis and dependency linting script to validate module import boundaries.
# Discover and execute the repository type-checking script across all provider submodules to confirm strict adherence to domain and API type contracts.
# Discover and execute the repository unit test suite covering client and mapper modules to verify payload normalization.
```

**Accept when:**
- All provider implementations encapsulate external network requests exclusively within dedicated client modules.
- Data transformation between provider schemas and canonical domain representations occurs solely within dedicated mapper modules.
- Automated architectural and type-checking verification scripts complete without boundary or dependency violations.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>