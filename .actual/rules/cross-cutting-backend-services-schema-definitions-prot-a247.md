---
version: 2
name: cross-cutting-backend-services-schema-definitions-prot-a247
description: "Adoption of @openstatus/regions for Canonical Region Definitions: Backend Services Schema Definitions Protocol Converters"
metadata:
  actual:
    adr_id: a24725fc-dc7a-4e97-a3e4-92c365affba7
    status: accepted
    candidate_group_id: e93ba485-cd34-462f-82e0-d234a52187de
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Adoption of @openstatus/regions for Canonical Region Definitions: Backend Services Schema Definitions Protocol Converters

Scope: applies to `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for all route schemas, validation layers, RPC handlers, and protocol converters dealing with region identifiers and metadata.

### Rules

- **R-REGION-001** MUST: Backend services, schema definitions, and protocol converters requiring geographical or infrastructure region definitions MUST import canonical region representations from the @openstatus/regions module.

### Verify

```bash
# Discover the workspace manifest and execute designated static type-checking and analysis tasks
# Discover and run the test suite to ensure schema validation and RPC converter mappings pass all contract tests
```

**Accept when:**
- All route schema definitions and RPC converters import region types from the canonical module without localized duplication.
- The discovered static analysis and build tasks pass with zero type errors related to region mapping.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated continuous integration checks executing repository type verification and static analysis, as well as peer code review auditing imports.
</enforcement>