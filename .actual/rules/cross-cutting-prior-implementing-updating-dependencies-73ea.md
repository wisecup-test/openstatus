---
version: 2
name: cross-cutting-prior-implementing-updating-dependencies-73ea
description: "Adoption of @openstatus/regions for Canonical Region Definitions: Prior Implementing Updating Dependencies Openstatus Regions"
metadata:
  actual:
    adr_id: 73ea020c-03d0-4c8a-acd5-58e9108e7df5
    status: accepted
    candidate_group_id: e93ba485-cd34-462f-82e0-d234a52187de
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Adoption of @openstatus/regions for Canonical Region Definitions: Prior Implementing Updating Dependencies Openstatus Regions

Scope: applies to `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for route schemas, validation layers, RPC handlers, and protocol converters dealing with region identifiers and metadata.

### Rules

- **R-REGION-001** MUST: Prior to implementing or updating dependencies on @openstatus/regions, consumers MUST inspect the project lock and resolution artifact to resolve the exact dependency version and verify API compatibility against documentation.
- **R-REGION-002** MUST: Locate the shared region module through repository workspace manifests and import canonical region types directly into route schemas and handler converters.
- **R-REGION-003** MUST: Consume exported region structures directly rather than re-declaring literal unions when defining input validation schemas or mapping RPC structures.

### Verify

```bash
# Discover workspace manifest and execute designated static type-checking and analysis tasks
# Discover and run test suite to ensure schema validation and RPC converter mappings pass all contract tests
```

**Accept when:**
- All route schema definitions and RPC converters import region types from the canonical module without localized duplication.
- The discovered static analysis and build tasks pass with zero type errors related to region mapping.

<enforcement>
Claude Code MUST NOT skip or defer verification. Pull requests containing locally declared region enums or duplicate string literals will fail review and must be refactored to use the canonical module.
</enforcement>