---
version: 2
name: cross-cutting-consumers-discover-project-dependency-ma-8ebb
description: "Effect Library Adoption for Status Fetcher Modules: Consumers Discover Project Dependency Manifest Lock"
metadata:
  actual:
    adr_id: 8ebbca20-34ed-42cf-b994-468ec8f36aa2
    status: accepted
    candidate_group_id: a1ad617f-2763-424d-96a8-0b772df61636
    evidence:
      source: deep
  scope:
    root: packages/status-fetcher
    paths:
      - src/fetchers/**
---

# Effect Library Adoption for Status Fetcher Modules: Consumers Discover Project Dependency Manifest Lock

Scope: applies to `packages/status-fetcher/src/fetchers/**`.

These rules are ALWAYS ACTIVE for all status fetcher modules implementing external provider status ingestion or health check data collection, and asynchronous pipelines responsible for fetching, parsing, and normalizing provider response payloads.

### Rules

- **R-EFF-001** MUST: Consumers MUST discover the project dependency manifest and lock artifact to resolve the exact pinned version of the Effect library prior to implementation.
- **R-EFF-002** MUST: Encapsulate external provider status fetch sequences inside Effect generator or pipe workflows.
- **R-EFF-003** MUST: Map boundary parsing and network communication errors into strongly typed domain error channels within the Effect program.

### Verify

```bash
# Discover and run the project build and typecheck script from the repository manifest
# Discover and run the project test suite runner covering the status fetchers
```

**Accept when:**
- All status fetcher modules compile without type errors under the resolved Effect library version.
- All status fetcher test suites execute and pass cleanly when evaluating effect execution paths.
- No unhandled Promise rejections or uncaught runtime exceptions escape fetcher execution boundaries.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated CI build pipelines, peer code reviews, and blocking of non-compliant pull requests ensure adherence to Effect composition and lockfile version grounding.
</enforcement>