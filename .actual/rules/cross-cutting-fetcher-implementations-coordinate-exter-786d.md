---
version: 2
name: cross-cutting-fetcher-implementations-coordinate-exter-786d
description: "Effect Library Adoption for Status Fetcher Modules: Fetcher Implementations Coordinate External Network Retrieval"
metadata:
  actual:
    adr_id: 786d3122-0efa-4e4d-9d71-ef8d70d163fd
    status: accepted
    candidate_group_id: a1ad617f-2763-424d-96a8-0b772df61636
    evidence:
      source: deep
  scope:
    root: packages/status-fetcher
    paths:
      - src/fetchers/**
---

# Effect Library Adoption for Status Fetcher Modules: Fetcher Implementations Coordinate External Network Retrieval

Scope: applies to `packages/status-fetcher/src/fetchers/**`.

These rules are ALWAYS ACTIVE for all status fetcher modules implementing external provider status ingestion or health check data collection.

### Rules

- **R-FETCH-001** MUST: Fetcher implementations MUST coordinate external network retrieval through internal shared fetch utilities rather than establishing independent unmanaged request pipelines.

### Verify

```bash
# Discover and execute the project build and typecheck script from the repository manifest
# Discover and execute the project test suite runner covering the status fetchers
```

**Accept when:**
- All status fetcher modules compile without type errors under the resolved Effect library version.
- All status fetcher test suites execute and pass cleanly when evaluating effect execution paths.
- No unhandled Promise rejections or uncaught runtime exceptions escape fetcher execution boundaries.

<enforcement>
Claude Code MUST NOT skip or defer verification. All status fetcher implementations must use Effect workflows and shared fetch utilities, verified through automated CI build pipelines and peer code review.
</enforcement>