---
version: 2
name: cross-cutting-external-status-data-ingestion-modules-b2ff
description: "Zod Schema Validation for External Service Ingestion: External Status Data Ingestion Modules Validate"
metadata:
  actual:
    adr_id: b2ffd8d5-db56-4aa1-83dd-b097c39801ee
    status: accepted
    candidate_group_id: 69aed0fa-e1bb-4853-925d-e29860e1a826
    evidence:
      source: deep
  scope:
    root: packages/status-fetcher
    paths:
      - src/fetchers/**
---

# Zod Schema Validation for External Service Ingestion: External Status Data Ingestion Modules Validate

Scope: applies to `packages/status-fetcher/src/fetchers/**`.

These rules are ALWAYS ACTIVE for all external status fetcher modules and HTTP client adapters ingesting data from third-party providers.

### Rules

- **R-ZOD-001** MUST: External status data ingestion modules MUST validate all incoming external payloads using Zod schemas prior to domain data mapping or internal propagation.

### Verify

```bash
# Discover and run the project type checker script defined in the dependency manifest
# Discover and run the test suite scripts defined in the repository to validate fetcher schema parsing against mock payloads
```

**Accept when:**
- The project type checker passes with zero type errors across all external fetcher modules.
- The project test suite passes, verifying that valid provider payloads parse successfully and malformed payloads trigger structured rejection errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. All external status payloads must enforce runtime schema validation.
</enforcement>