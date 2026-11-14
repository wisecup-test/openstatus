---
version: 2
name: cross-cutting-consumers-referencing-external-library-d-5910
description: "Data Access Dispatch via the Internal fetchers Module: Consumers Referencing External Library Dependencies Within"
metadata:
  actual:
    adr_id: 59109a39-cb22-4e8c-886d-a2d8041d79d7
    status: accepted
    candidate_group_id: ab4bb405-8fa9-4272-8393-c81e5691d4c3
    evidence:
      source: deep
  scope:
    root: packages/status-fetcher
    paths:
      - scripts/**
---

# Data Access Dispatch via the Internal fetchers Module: Consumers Referencing External Library Dependencies Within

Scope: applies to `packages/status-fetcher/scripts/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-FETCH-001** MUST: Consumers referencing external library dependencies within fetcher implementations MUST discover the project lock file and resolve the exact locked version before invoking external APIs.

### Verify

```bash
# Discover and run the project test suite using the active environment test runner to verify fetcher dispatch and canHandle behavior.
# Discover and execute the repository static type verification script to ensure fetcher interface compliance.
```

**Accept when:**
- All registered fetchers satisfy the common interface and pass test suite assertions.
- Entry dispatch reliably resolves the compatible fetcher without unhandled lookup rejections.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>