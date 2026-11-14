---
version: 2
name: cross-cutting-multi-phase-import-writers-maintain-3669
description: "Adoption of @openstatus/importers for External Provider Data Ingestion: Multi Phase Import Writers Maintain Memory"
metadata:
  actual:
    adr_id: 36690aa9-e5e3-46eb-ad54-074f9850408a
    status: accepted
    candidate_group_id: 09c9bc24-7a08-4bec-ac7b-e52f23f88a2c
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/import/**
---

# Adoption of @openstatus/importers for External Provider Data Ingestion: Multi Phase Import Writers Maintain Memory

Scope: applies to `packages/services/src/import/**`.

These rules are ALWAYS ACTIVE for all service workflows responsible for importing external monitoring, component, or status page resources, preview validation, dry-run evaluations, and quota limit checks for inbound platform data migrations, and multi-phase database persistence routines mapping external provider structures to internal schemas.

### Rules

- **R-ADP-001** SHOULD: Multi-phase import writers maintain in-memory identity mapping structures linking external source identifiers to newly assigned platform identifiers to ensure referential integrity across persistence stages.

### Verify

```bash
# Discover and run the project's static analysis and dependency linting suite to ensure all external provider parsing is confined to the importers module.
# Discover and execute the automated test suites covering import preview validation, limit enforcement, and multi-phase persistence workflows.
```

**Accept when:**
- All external provider integration workflows route schema mapping and validation through the dedicated importers module.
- Transactional persistence workflows derive tenancy identifiers exclusively from authenticated context rather than provider payloads.
- Automated verification test suites pass for import preview validation, limit enforcement, and multi-phase persistence.

<enforcement>
Claude Code MUST NOT skip or defer verification. All external provider parsing, multi-phase imports, and limit evaluations must strictly comply with the defined module boundaries and identity mapping requirements.
</enforcement>