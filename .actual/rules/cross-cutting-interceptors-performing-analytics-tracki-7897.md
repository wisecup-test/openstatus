---
version: 2
name: cross-cutting-interceptors-performing-analytics-tracki-7897
description: "@connectrpc/connect Interceptor Pipeline Architecture: Interceptors Performing Analytics Tracking Non Critical"
metadata:
  actual:
    adr_id: 789727ad-2729-491a-8501-b7b0fd876ada
    status: accepted
    candidate_group_id: 4d15d7b7-58ac-4c28-ae26-f4b6181018d4
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/rpc/interceptors/**
---

# @connectrpc/connect Interceptor Pipeline Architecture: Interceptors Performing Analytics Tracking Non Critical

Scope: applies to `apps/server/src/routes/rpc/interceptors/**`.

These rules are ALWAYS ACTIVE for all inbound and outbound RPC service procedures handled through the @connectrpc/connect server routing layer and cross-cutting request lifecycle handlers.

### Rules

- **R-INT-001** SHOULD: Interceptors performing analytics tracking or non-critical telemetry SHOULD handle tracking failures gracefully without aborting the underlying RPC request pipeline.

### Verify

```bash
# Discover and run the project static analysis and type checking suite to verify @connectrpc/connect interceptor type signatures
# Discover and execute the automated test runner to run unit and integration tests covering the RPC interceptor pipeline
```

**Accept when:**
- Static type analysis verifies that all interceptors strictly conform to @connectrpc/connect interceptor signatures without type errors
- Automated test suites covering RPC interceptors pass successfully, verifying that logging, error handling, and context propagation execute as specified

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration checks executing repository test and static analysis suites, and mandatory architectural peer reviews.
</enforcement>