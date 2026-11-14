---
version: 2
name: cross-cutting-data-fetching-components-dispatch-target-d67d
description: "Data Access Dispatch via the Internal fetchers Module: Data Fetching Components Dispatch Target Queries"
metadata:
  actual:
    adr_id: d67d5fd4-46b2-4417-bac9-320c4d14d5f8
    status: accepted
    candidate_group_id: ab4bb405-8fa9-4272-8393-c81e5691d4c3
    evidence:
      source: deep
  scope:
    root: packages/status-fetcher
    paths:
      - scripts/**
---

# Data Access Dispatch via the Internal fetchers Module: Data Fetching Components Dispatch Target Queries

Scope: applies to `packages/status-fetcher/scripts/**`.

These rules are ALWAYS ACTIVE for data retrieval operations dispatched across heterogeneous entries within the status fetching subsystem.

### Rules

- **R-DF-001** MUST: Data fetching components MUST dispatch target queries through the fetchers module by selecting the appropriate handler via the canHandle predicate.

### Verify

```bash
# Discover and run the project test suite using the active environment test runner to verify fetcher dispatch and canHandle behavior.
# Discover and execute the repository static type verification script to ensure fetcher interface compliance.
```

**Accept when:**
- All registered fetchers satisfy the common interface and pass test suite assertions.
- Entry dispatch reliably resolves the compatible fetcher without unhandled lookup rejections.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration test passes validating fetcher resolution and canHandle execution, as well as peer code review, are mandatory.
</enforcement>