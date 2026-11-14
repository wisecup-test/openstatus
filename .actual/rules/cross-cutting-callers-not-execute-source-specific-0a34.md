---
version: 2
name: cross-cutting-callers-not-execute-source-specific-0a34
description: "Data Access Dispatch via the Internal fetchers Module: Callers Not Execute Source Specific Data"
metadata:
  actual:
    adr_id: 0a34eefd-b912-46e5-9e5d-ecac6caca54e
    status: accepted
    candidate_group_id: ab4bb405-8fa9-4272-8393-c81e5691d4c3
    evidence:
      source: deep
  scope:
    root: packages/status-fetcher
    paths:
      - scripts/**
---

# Data Access Dispatch via the Internal fetchers Module: Callers Not Execute Source Specific Data

Scope: applies to `packages/status-fetcher/scripts/**`.

These rules are ALWAYS ACTIVE for data retrieval operations dispatched across heterogeneous entries within the status fetching subsystem.

### Rules

- **R-FETCH-001** MUST_NOT: Callers MUST NOT execute source-specific data fetching operations by directly instantiating or coupling to concrete fetcher classes.

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