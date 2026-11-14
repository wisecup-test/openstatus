---
version: 2
name: cross-cutting-every-external-integration-provider-deco-1d4c
description: "Importer Provider Client and Mapper Submodule Architecture: Every External Integration Provider Decompose Its"
metadata:
  actual:
    adr_id: 1d4c614b-d3db-4c55-97a3-4b732ee91591
    status: accepted
    candidate_group_id: 1c1bfc9d-9f02-4f19-ba60-a13c7cefdafb
    evidence:
      source: deep
  scope:
    root: packages/importers
    paths:
      - src/providers/**
---

# Importer Provider Client and Mapper Submodule Architecture: Every External Integration Provider Decompose Its

Scope: applies to `packages/importers/src/providers/**`.

These rules are ALWAYS ACTIVE for all external service integration providers within the data importer subsystem.

### Rules

- **R-IMP-001** MUST: Every external integration provider MUST decompose its implementation into isolated internal modules dedicated respectively to provider orchestration, remote transport client interaction, schema mapping, and external API type definitions.

### Verify

```bash
# Discover and execute repository static analysis and dependency linting script to validate module import boundaries
# Discover and execute repository type-checking script across all provider submodules
# Discover and execute repository unit test suite covering client and mapper modules
```

**Accept when:**
- All provider implementations encapsulate external network requests exclusively within dedicated client modules.
- Data transformation between provider schemas and canonical domain representations occurs solely within dedicated mapper modules.
- Automated architectural and type-checking verification scripts complete without boundary or dependency violations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration dependency boundary analysis, static type checking, and peer code review strictly enforce these provider module separations.
</enforcement>