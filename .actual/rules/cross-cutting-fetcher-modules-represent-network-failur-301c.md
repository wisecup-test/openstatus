---
version: 2
name: cross-cutting-fetcher-modules-represent-network-failur-301c
description: "Effect Library Adoption for Status Fetcher Modules: Fetcher Modules Represent Network Failures Boundary"
metadata:
  actual:
    adr_id: 301cc68b-0923-487c-b253-b725db72f0bd
    status: accepted
    candidate_group_id: a1ad617f-2763-424d-96a8-0b772df61636
    evidence:
      source: deep
  scope:
    root: packages/status-fetcher
    paths:
      - src/fetchers/**
---

# Effect Library Adoption for Status Fetcher Modules: Fetcher Modules Represent Network Failures Boundary

Scope: applies to `packages/status-fetcher/src/fetchers/**`.

These rules are ALWAYS ACTIVE for all status fetcher modules implementing external provider status ingestion or health check data collection.

### Rules

- **R-EFF-001** MUST: Fetcher modules MUST represent network failures and boundary parsing anomalies as typed errors within the Effect error channel rather than allowing unhandled exceptions to escape.

### Verify

```bash
# Discover the project build and typecheck script from the repository manifest and execute it against the affected packages
# Discover the project test suite runner from the repository manifest and execute all unit and integration tests covering the status fetchers
```

**Accept when:**
- All status fetcher modules compile without type errors under the resolved Effect library version.
- All status fetcher test suites execute and pass cleanly when evaluating effect execution paths.
- No unhandled Promise rejections or uncaught runtime exceptions escape fetcher execution boundaries.

<enforcement>
Claude Code MUST NOT skip or defer verification. All status fetcher implementations must use Effect pipelines with strongly typed error channels.
</enforcement>