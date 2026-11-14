---
version: 2
name: cross-cutting-before-adopting-updating-any-versioned-667c
description: "Importer Provider Client and Mapper Submodule Architecture: Before Adopting Updating Any Versioned External"
metadata:
  actual:
    adr_id: 667cdce8-277c-4731-9c29-e5696477f6a3
    status: accepted
    candidate_group_id: 1c1bfc9d-9f02-4f19-ba60-a13c7cefdafb
    evidence:
      source: deep
  scope:
    root: packages/importers
    paths:
      - src/providers/**
---

# Importer Provider Client and Mapper Submodule Architecture: Before Adopting Updating Any Versioned External

Scope: applies to `packages/importers/src/providers/**`.

These rules are ALWAYS ACTIVE for all external service integration providers within the data importer subsystem and module implementations handling external API ingestion, normalization, and state indexing.

### Rules

- **R-IMP-001** MUST: Before adopting or updating any versioned external dependency utilized by provider modules, the consumer MUST inspect the repository lock artifact to determine the exact resolved dependency version and validate that referenced APIs exist within that specific version reference.

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