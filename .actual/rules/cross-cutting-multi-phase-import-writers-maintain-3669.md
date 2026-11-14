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

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OPN-001** SHOULD: Multi-phase import writers SHOULD maintain in-memory identity mapping structures linking external source identifiers to newly assigned platform identifiers to ensure referential integrity across persistence stages.

### Verify

```bash
# Discover and run the project's static analysis and dependency linting suite
# Discover and execute the automated test suites covering import preview validation, limit enforcement, and multi-phase persistence workflows
```

**Accept when:**
- All external provider integration workflows route schema mapping and validation through the dedicated importers module.
- Transactional persistence workflows derive tenancy identifiers exclusively from authenticated context rather than provider payloads.
- Automated verification test suites pass for import preview validation, limit enforcement, and multi-phase persistence.

<enforcement>
Claude Code MUST NOT skip or defer verification. All external provider parsing, limit enforcement, and multi-phase identity mapping must strictly adhere to the designated importer boundaries and transactional verification standards.
</enforcement>