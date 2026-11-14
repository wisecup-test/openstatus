---
version: 2
name: cross-cutting-external-fetcher-implementations-handle-2d9a
description: "Zod Schema Validation for External Service Ingestion: External Fetcher Implementations Handle Schema Validation"
metadata:
  actual:
    adr_id: 2d9a30bc-1c61-483c-a4e8-fc016a46ab52
    status: accepted
    candidate_group_id: 69aed0fa-e1bb-4853-925d-e29860e1a826
    evidence:
      source: deep
  scope:
    root: packages/status-fetcher
    paths:
      - src/fetchers/**
---

# Zod Schema Validation for External Service Ingestion: External Fetcher Implementations Handle Schema Validation

Scope: applies to `packages/status-fetcher/src/fetchers/**`.

These rules are ALWAYS ACTIVE for all external status fetcher modules and HTTP client adapters ingesting data from third-party providers.

### Rules

- **R-EXT-001** MUST: External fetcher implementations MUST handle schema validation failures explicitly at the ingestion boundary and MUST NOT allow unvalidated raw payloads to cross into internal domain workflows.

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