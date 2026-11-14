---
version: 2
name: cross-cutting-service-orchestration-workflows-validate-d414
description: "Adoption of @openstatus/importers for External Provider Data Ingestion: Service Orchestration Workflows Validate Provider Configuration"
metadata:
  actual:
    adr_id: d414cf28-4c81-482c-b88d-39321e76dd89
    status: accepted
    candidate_group_id: 09c9bc24-7a08-4bec-ac7b-e52f23f88a2c
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/import/**
---

# Adoption of @openstatus/importers for External Provider Data Ingestion: Service Orchestration Workflows Validate Provider Configuration

Scope: applies to `packages/services/src/import/**`.

These rules are ALWAYS ACTIVE for all service workflows responsible for importing external monitoring, component, or status page resources, preview validation, dry-run evaluations, and quota limit checks for inbound platform data migrations, and multi-phase database persistence routines mapping external provider structures to internal schemas.

### Rules

- **R-OPN-001** MUST: Service orchestration workflows MUST validate provider configuration and execute dry-run previews using the standardized provider validation interface prior to triggering persistent import jobs.

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
Claude Code MUST NOT skip or defer verification. All external provider ingestion must be validated against the specified modules and verification suites must pass successfully.
</enforcement>