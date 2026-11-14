---
version: 2
name: cross-cutting-schema-definitions-external-payloads-enf-9106
description: "Zod Schema Validation for External Service Ingestion: Schema Definitions External Payloads Enforce Strict"
metadata:
  actual:
    adr_id: 910683f7-c188-488c-865a-3ecf295cfbd5
    status: accepted
    candidate_group_id: 69aed0fa-e1bb-4853-925d-e29860e1a826
    evidence:
      source: deep
  scope:
    root: packages/status-fetcher
    paths:
      - src/fetchers/**
---

# Zod Schema Validation for External Service Ingestion: Schema Definitions External Payloads Enforce Strict

Scope: applies to `packages/status-fetcher/src/fetchers/**`.

These rules are ALWAYS ACTIVE for all external status fetcher modules and HTTP client adapters ingesting data from third-party providers.

### Rules

- **R-EXT-001** MUST: Schema definitions for external payloads MUST enforce strict property types using z.object, z.string, z.enum, and built-in format refinements such as URL and datetime validation.

### Verify

```bash
# Discover and run the project type checker script defined in the dependency manifest to verify schema type compatibility.
# Discover and run the test suite scripts defined in the repository to validate that fetcher schema parsing passes against mock status payloads.
```

**Accept when:**
- The project type checker passes with zero type errors across all external fetcher modules.
- The project test suite passes, verifying that valid provider payloads parse successfully and malformed payloads trigger structured rejection errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>