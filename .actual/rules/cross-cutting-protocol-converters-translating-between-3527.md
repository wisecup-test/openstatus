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

- **R-REGION-001** SHOULD: Protocol converters translating between external representations and internal transport contracts SHOULD delegate region normalization and mapping to functions and types exported by @openstatus/regions.

### Verify

```bash
# Discover the workspace manifest and execute designated static type-checking/analysis tasks
# Discover and run the test suite to ensure schema validation and RPC converter mappings pass all contract tests
```

**Accept when:**
- All route schema definitions and RPC converters import region types from the canonical module without localized duplication.
- The discovered static analysis and build tasks pass with zero type errors related to region mapping.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is enforced via automated CI checks and peer code reviews.
</enforcement>