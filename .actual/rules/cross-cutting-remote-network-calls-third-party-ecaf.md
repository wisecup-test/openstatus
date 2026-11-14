---
version: 2
name: cross-cutting-remote-network-calls-third-party-ecaf
description: "Importer Provider Client and Mapper Submodule Architecture: Remote Network Calls Third Party Endpoints"
metadata:
  actual:
    adr_id: ecaf6046-8384-473a-bb99-50af8e813646
    status: accepted
    candidate_group_id: 1c1bfc9d-9f02-4f19-ba60-a13c7cefdafb
    evidence:
      source: deep
  scope:
    root: packages/importers
    paths:
      - src/providers/**
---

# Importer Provider Client and Mapper Submodule Architecture: Remote Network Calls Third Party Endpoints

Scope: applies to `packages/importers/src/providers/**`.

These rules are ALWAYS ACTIVE for all external service integration providers within the data importer subsystem and module implementations handling external API ingestion, normalization, and state indexing.

### Rules

- **R-IMP-001** MUST: Remote network calls to third-party endpoints MUST be encapsulated exclusively within the dedicated client module and MUST NOT be invoked directly from mapper or domain orchestration modules.

### Verify

```bash
# Discover and execute repository static analysis and dependency linting script
# Discover and execute repository type-checking script across provider submodules
# Discover and execute repository unit test suite covering client and mapper modules
```

**Accept when:**
- All provider implementations encapsulate external network requests exclusively within dedicated client modules.
- Data transformation between provider schemas and canonical domain representations occurs solely within dedicated mapper modules.
- Automated architectural and type-checking verification scripts complete without boundary or dependency violations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration builds fail upon detection of undecomposed or circular provider imports, and code reviews block merges that bypass mapper or client module boundaries.
</enforcement>