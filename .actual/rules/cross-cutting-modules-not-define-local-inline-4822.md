---
version: 2
name: cross-cutting-modules-not-define-local-inline-4822
description: "Adoption of @openstatus/regions for Canonical Region Definitions: Modules Not Define Local Inline String"
metadata:
  actual:
    adr_id: 4822f0be-2c8a-4b17-a125-0543da21c2d6
    status: accepted
    candidate_group_id: e93ba485-cd34-462f-82e0-d234a52187de
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Adoption of @openstatus/regions for Canonical Region Definitions: Modules Not Define Local Inline String

Scope: applies to `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for all route schemas, validation layers, RPC handlers, and protocol converters dealing with region identifiers and metadata.

### Rules

- **R-REG-001** MUST_NOT: Modules MUST_NOT define local, inline string literals or disjoint custom enumerations for region identifiers that duplicate entities provided by @openstatus/regions.

### Verify

```bash
# Discover the workspace manifest and execute the designated static type-checking and analysis tasks to verify region import conformance.
# Discover and run the test suite to ensure schema validation and RPC converter mappings pass all contract tests without type mismatch.
```

**Accept when:**
- All route schema definitions and RPC converters import region types from the canonical module without localized duplication.
- The discovered static analysis and build tasks pass with zero type errors related to region mapping.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>