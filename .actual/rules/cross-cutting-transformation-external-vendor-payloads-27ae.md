---
version: 2
name: cross-cutting-transformation-external-vendor-payloads-27ae
description: "Importer Provider Client and Mapper Submodule Architecture: Transformation External Vendor Payloads Canonical Domain"
metadata:
  actual:
    adr_id: 27ae6a0d-c7ca-4fde-a649-31bb9d837be6
    status: accepted
    candidate_group_id: 1c1bfc9d-9f02-4f19-ba60-a13c7cefdafb
    evidence:
      source: deep
  scope:
    root: packages/importers
    paths:
      - src/providers/**
---

# Importer Provider Client and Mapper Submodule Architecture: Transformation External Vendor Payloads Canonical Domain

Scope: applies to `packages/importers/src/providers/**`.

These rules are ALWAYS ACTIVE for all external service integration providers within the data importer subsystem and module implementations handling external API ingestion, normalization, and state indexing.

### Rules

- **R-IMP-001** MUST: Transformation from external vendor payloads to canonical domain formats MUST be performed exclusively within the dedicated mapper module.

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
Claude Code MUST NOT skip or defer verification. Continuous integration builds fail upon detection of undecomposed or circular provider imports, and code reviews block merges that bypass mapper or client module boundaries.
</enforcement>