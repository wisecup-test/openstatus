---
version: 2
name: cross-cutting-before-implementing-extending-dependenci-3390
description: "Adoption of @openstatus/importers for External Provider Data Ingestion: Before Implementing Extending Dependencies Importers Module"
metadata:
  actual:
    adr_id: 3390c964-1c7b-49d8-9a46-c8d1d7657885
    status: accepted
    candidate_group_id: 09c9bc24-7a08-4bec-ac7b-e52f23f88a2c
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/import/**
---

# Adoption of @openstatus/importers for External Provider Data Ingestion: Before Implementing Extending Dependencies Importers Module

Scope: applies to `packages/services/src/import/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-IMP-001** MUST: Before implementing or extending dependencies on the importers module, the consumer MUST inspect the repository lock artifact to determine the exact resolved dependency versions and verify API compatibility against authoritative reference documentation.

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
Claude Code MUST NOT skip or defer verification.
</enforcement>