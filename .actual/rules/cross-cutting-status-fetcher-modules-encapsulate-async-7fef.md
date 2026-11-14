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

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SF-001** MUST: Status fetcher modules MUST encapsulate asynchronous data retrieval and status aggregation pipelines within the Effect library effect system.

### Verify

```bash
# Discover the project build, typecheck, and test runner scripts from the repository manifest and execute them
npm run build
npm run test
```

**Accept when:**
- All status fetcher modules compile without type errors under the resolved Effect library version.
- All status fetcher test suites execute and pass cleanly when evaluating effect execution paths.
- No unhandled Promise rejections or uncaught runtime exceptions escape fetcher execution boundaries.

<enforcement>
Claude Code MUST NOT skip or defer verification. All status fetcher modules must be verified via automated type checking and test suites ensuring proper Effect encapsulation.
</enforcement>