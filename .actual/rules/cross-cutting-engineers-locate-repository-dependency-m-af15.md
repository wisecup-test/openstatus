---
version: 2
name: cross-cutting-engineers-locate-repository-dependency-m-af15
description: "Zod Schema Validation for External Service Ingestion: Engineers Locate Repository Dependency Manifest Lock"
metadata:
  actual:
    adr_id: af15a281-86eb-4ba3-a959-636aa26ff752
    status: accepted
    candidate_group_id: 69aed0fa-e1bb-4853-925d-e29860e1a826
    evidence:
      source: deep
  scope:
    root: packages/status-fetcher
    paths:
      - src/fetchers/**
---

# Zod Schema Validation for External Service Ingestion: Engineers Locate Repository Dependency Manifest Lock

Scope: applies to `packages/status-fetcher/src/fetchers/**`.

These rules are ALWAYS ACTIVE for all external status fetcher modules and HTTP client adapters ingesting data from third-party providers.

### Rules

- **R-ZOD-001** MUST: Engineers MUST locate the repository dependency manifest and lock resolution artifact to verify the exact resolved version of the validation library before implementing or updating schema definitions.
- **R-ZOD-002** MUST: Infer static TypeScript types directly from validation schemas to maintain strict parity between runtime validation and static typing.
- **R-ZOD-003** MUST: Keep external provider schemas scoped to their respective fetcher modules to avoid cross-provider contract coupling.

### Verify

```bash
# Discover and run the project type checker script defined in the dependency manifest
# Discover and run the test suite scripts defined in the repository to validate fetcher schema parsing
```

**Accept when:**
- The project type checker passes with zero type errors across all external fetcher modules.
- The project test suite passes, verifying that valid provider payloads parse successfully and malformed payloads trigger structured rejection errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>