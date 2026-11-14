---
version: 2
name: cross-cutting-database-persistence-routines-assign-wor-3fbb
description: "Adoption of @openstatus/importers for External Provider Data Ingestion: Database Persistence Routines Assign Workspace Tenancy"
metadata:
  actual:
    adr_id: 3fbb63f3-8e65-462f-b8cf-04535fc9a7be
    status: accepted
    candidate_group_id: 09c9bc24-7a08-4bec-ac7b-e52f23f88a2c
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/import/**
---

# Adoption of @openstatus/importers for External Provider Data Ingestion: Database Persistence Routines Assign Workspace Tenancy

Scope: applies to `packages/services/src/import/**`.

These rules are ALWAYS ACTIVE for all service workflows responsible for importing external monitoring, component, or status page resources, preview validation, dry-run evaluations, and quota limit checks for inbound platform data migrations, and multi-phase database persistence routines mapping external provider structures to internal schemas.

### Rules

- **R-IMP-001** MUST: Database persistence routines MUST assign workspace and tenancy identifiers derived strictly from authenticated execution context and MUST NOT accept tenancy identifiers from external provider payload data.

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