---
version: 2
name: cross-cutting-status-fetcher-modules-encapsulate-async-7fef
description: "Effect Library Adoption for Status Fetcher Modules: Status Fetcher Modules Encapsulate Asynchronous Data"
metadata:
  actual:
    adr_id: 7fef1677-bd9e-4177-b9c3-c3ca3c769940
    status: accepted
    candidate_group_id: a1ad617f-2763-424d-96a8-0b772df61636
    evidence:
      source: deep
  scope:
    root: packages/status-fetcher
    paths:
      - src/fetchers/**
---

# Effect Library Adoption for Status Fetcher Modules: Status Fetcher Modules Encapsulate Asynchronous Data

Scope: applies to `packages/status-fetcher/src/fetchers/**`.

These rules are ALWAYS ACTIVE for all status fetcher modules implementing external provider status ingestion or health check data collection, and asynchronous pipelines responsible for fetching, parsing, and normalizing provider response payloads.

### Rules

- **R-STATUS-001** MUST: Status fetcher modules MUST encapsulate asynchronous data retrieval and status aggregation pipelines within the Effect library effect system.

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
Claude Code MUST NOT skip or defer verification. Automated CI build pipelines and peer code reviews verify proper encapsulation of asynchronous operations within Effect workflows, and pull requests containing raw unmanaged async executions or unhandled exception patterns in fetcher modules are blocked from merging.
</enforcement>