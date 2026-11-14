---
version: 2
name: cross-cutting-external-provider-payloads-validated-nor-c5d2
description: "Effect Library Adoption for Status Fetcher Modules: External Provider Payloads Validated Normalized Within"
metadata:
  actual:
    adr_id: c5d25f99-b1f4-4060-8af6-1dfe6a4d1024
    status: accepted
    candidate_group_id: a1ad617f-2763-424d-96a8-0b772df61636
    evidence:
      source: deep
  scope:
    root: packages/status-fetcher
    paths:
      - src/fetchers/**
---

# Effect Library Adoption for Status Fetcher Modules: External Provider Payloads Validated Normalized Within

Scope: applies to `packages/status-fetcher/src/fetchers/**`.

These rules are ALWAYS ACTIVE for all status fetcher modules implementing external provider status ingestion or health check data collection.

### Rules

- **R-EFF-001** SHOULD: External provider payloads SHOULD be validated and normalized within the effect pipeline before constructing aggregate status representations.

### Verify

```bash
# Discover the project build and typecheck script from the repository manifest and execute it against the affected packages.
# Discover the project test suite runner from the repository manifest and execute all unit and integration tests covering the status fetchers.
```

**Accept when:**
- All status fetcher modules compile without type errors under the resolved Effect library version.
- All status fetcher test suites execute and pass cleanly when evaluating effect execution paths.
- No unhandled Promise rejections or uncaught runtime exceptions escape fetcher execution boundaries.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification by automated CI build pipelines, type safety, and test coverage is mandatory.
</enforcement>