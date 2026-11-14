---
version: 2
name: cross-cutting-fetcher-modules-not-execute-unmanaged-a137
description: "Effect Library Adoption for Status Fetcher Modules: Fetcher Modules Not Execute Unmanaged Asynchronous"
metadata:
  actual:
    adr_id: a1370b01-fb2c-424d-879f-35d139de1391
    status: accepted
    candidate_group_id: a1ad617f-2763-424d-96a8-0b772df61636
    evidence:
      source: deep
  scope:
    root: packages/status-fetcher
    paths:
      - src/fetchers/**
---

# Effect Library Adoption for Status Fetcher Modules: Fetcher Modules Not Execute Unmanaged Asynchronous

Scope: applies to `packages/status-fetcher/src/fetchers/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-FETCH-001** MUST_NOT: Fetcher modules MUST NOT execute unmanaged asynchronous operations or raw background promises outside of supervised Effect primitives.

### Verify

```bash
# Discover and run the project build/typecheck script from the repository manifest against affected packages
# Discover and run the project test suite runner covering the status fetchers
```

**Accept when:**
- All status fetcher modules compile without type errors under the resolved Effect library version.
- All status fetcher test suites execute and pass cleanly when evaluating effect execution paths.
- No unhandled Promise rejections or uncaught runtime exceptions escape fetcher execution boundaries.

<enforcement>
Claude Code MUST NOT skip or defer verification. All status fetcher modules must enforce Effect supervision for asynchronous operations.
</enforcement>