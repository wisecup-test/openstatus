---
version: 2
name: cross-cutting-validation-schemas-declare-optional-fiel-ddcd
description: "Zod Schema Validation for External Service Ingestion: Validation Schemas Declare Optional Fields Explicitly"
metadata:
  actual:
    adr_id: ddcd3800-a9e3-4d1e-ad86-1866aa1f7255
    status: accepted
    candidate_group_id: 69aed0fa-e1bb-4853-925d-e29860e1a826
    evidence:
      source: deep
  scope:
    root: packages/status-fetcher
    paths:
      - src/fetchers/**
---

# Zod Schema Validation for External Service Ingestion: Validation Schemas Declare Optional Fields Explicitly

Scope: applies to `packages/status-fetcher/src/fetchers/**`.

These rules are ALWAYS ACTIVE for all external status fetcher modules and HTTP client adapters ingesting data from third-party providers.

### Rules

- **R-ZOD-001** SHOULD: Validation schemas SHOULD declare optional fields explicitly using schema modifier methods to prevent missing property errors while maintaining strict validation for defined fields.

### Verify

```bash
# Discover and run the project type checker script defined in the dependency manifest
# Discover and run the test suite scripts defined in the repository
```

**Accept when:**
- The project type checker passes with zero type errors across all external fetcher modules.
- The project test suite passes, verifying that valid provider payloads parse successfully and malformed payloads trigger structured rejection errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. All incoming external status payloads must define runtime validation schemas with explicitly declared optional fields.
</enforcement>