---
version: 2
name: cross-cutting-import-execution-pipelines-evaluate-phas-394e
description: "Adoption of @openstatus/importers for External Provider Data Ingestion: Import Execution Pipelines Evaluate Phase Summary"
metadata:
  actual:
    adr_id: 394e455e-eee7-4e8f-bb54-ef6634d9e3eb
    status: accepted
    candidate_group_id: 09c9bc24-7a08-4bec-ac7b-e52f23f88a2c
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/import/**
---

# Adoption of @openstatus/importers for External Provider Data Ingestion: Import Execution Pipelines Evaluate Phase Summary

Scope: applies to `packages/services/src/import/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-IMP-001** SHOULD: Import execution pipelines SHOULD evaluate phase summary totals against account plan limits before committing transactional resource insertions.
- **R-IMP-002** MANDATORY: External provider integrations MUST use project-derived dependency manifests, lock files, and verified public API documentation for version resolution.
- **R-IMP-003** MANDATORY: Phase writers MUST retain an in-memory source-to-internal identifier mapping structure to resolve relational dependencies across stages.
- **R-IMP-004** MANDATORY: Dry-run execution logic MUST be encapsulated within provider validation interfaces to support idempotent preview workflows.

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
Claude Code MUST NOT skip or defer verification. Compliance is verified via automated dependency boundary checks, peer reviews, and integration test suites.
</enforcement>