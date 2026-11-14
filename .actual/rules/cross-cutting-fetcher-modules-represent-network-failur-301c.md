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
# Discover and execute project build and typecheck script from manifest
# Discover and execute test suite runner for status fetchers
```

**Accept when:**
- All status fetcher modules compile without type errors under the resolved Effect library version.
- All status fetcher test suites execute and pass cleanly when evaluating effect execution paths.
- No unhandled Promise rejections or uncaught runtime exceptions escape fetcher execution boundaries.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated CI build pipelines, peer code reviews, and blocking of pull requests with raw unmanaged async executions enforce this rule.
</enforcement>