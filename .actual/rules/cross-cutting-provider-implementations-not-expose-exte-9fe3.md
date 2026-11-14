---
version: 2
name: cross-cutting-provider-implementations-not-expose-exte-9fe3
description: "Importer Provider Client and Mapper Submodule Architecture: Provider Implementations Not Expose External Vendor"
metadata:
  actual:
    adr_id: 9fe3991b-8baa-40cf-94a6-7d43fe99d41f
    status: accepted
    candidate_group_id: 1c1bfc9d-9f02-4f19-ba60-a13c7cefdafb
    evidence:
      source: deep
  scope:
    root: packages/importers
    paths:
      - src/providers/**
---

# Importer Provider Client and Mapper Submodule Architecture: Provider Implementations Not Expose External Vendor

Scope: applies to `packages/importers/src/providers/**`.

These rules are ALWAYS ACTIVE for all external service integration providers within the data importer subsystem and module implementations handling external API ingestion, normalization, and state indexing.

### Rules

- **R-PRV-001** MUST_NOT: Provider implementations MUST NOT expose external vendor API types beyond their local module boundary, returning only canonical domain types to consuming packages.

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