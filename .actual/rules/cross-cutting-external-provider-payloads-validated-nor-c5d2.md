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

- **R-EFF-001** SHOULD: External provider payloads be validated and normalized within the effect pipeline before constructing aggregate status representations.
- **R-EFF-002** MANDATORY: Encapsulate external provider status fetch sequences inside Effect generator or pipe workflows.
- **R-EFF-003** MANDATORY: Map boundary parsing and network communication errors into strongly typed domain error channels within the Effect program.

### Verify

```bash
# Discover and execute the project build and typecheck script from the repository manifest against affected packages
# Discover and execute the project test suite runner covering the status fetchers
```

**Accept when:**
- All status fetcher modules compile without type errors under the resolved Effect library version.
- All status fetcher test suites execute and pass cleanly when evaluating effect execution paths.
- No unhandled Promise rejections or uncaught runtime exceptions escape fetcher execution boundaries.

<enforcement>
Claude Code MUST NOT skip or defer verification. All status fetcher modules implementing external provider status ingestion must strictly use Effect pipelines with validated and normalized payloads, and automated CI pipelines and peer reviews will block violations.
</enforcement>