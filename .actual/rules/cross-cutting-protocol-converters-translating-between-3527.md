---
version: 2
name: cross-cutting-protocol-converters-translating-between-3527
description: "Adoption of @openstatus/regions for Canonical Region Definitions: Protocol Converters Translating Between External Representations"
metadata:
  actual:
    adr_id: 35274827-07e8-4f34-8357-49879b3f3779
    status: accepted
    candidate_group_id: e93ba485-cd34-462f-82e0-d234a52187de
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Adoption of @openstatus/regions for Canonical Region Definitions: Protocol Converters Translating Between External Representations

Scope: applies to `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for route schemas, validation layers, RPC handlers, and protocol converters dealing with region identifiers and metadata.

### Rules

- **R-OPN-001** SHOULD: Protocol converters translating between external representations and internal transport contracts delegate region normalization and mapping to functions and types exported by @openstatus/regions.

### Verify

```bash
# Discover workspace manifest, run type-checking and analysis tasks, and run the test suite
# (Commands must be discovered from repository configuration)
```

**Accept when:**
- All route schema definitions and RPC converters import region types from the canonical module without localized duplication.
- The discovered static analysis and build tasks pass with zero type errors related to region mapping.
- The test suite ensures schema validation and RPC converter mappings pass all contract tests without type mismatch.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks execute repository type verification and static analysis, and peer code review audits imports in new and modified route schemas and RPC converters.
</enforcement>