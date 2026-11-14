---
version: 2
name: cross-cutting-cross-cutting-rpc-request-lifecycle-e53b
description: "@connectrpc/connect Interceptor Pipeline Architecture: Cross Cutting Rpc Request Lifecycle Concerns"
metadata:
  actual:
    adr_id: e53bdade-3cd9-4728-8e83-2471b9e8eb71
    status: accepted
    candidate_group_id: 4d15d7b7-58ac-4c28-ae26-f4b6181018d4
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/rpc/interceptors/**
---

# @connectrpc/connect Interceptor Pipeline Architecture: Cross Cutting Rpc Request Lifecycle Concerns

Scope: applies to `apps/server/src/routes/rpc/interceptors/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CONNECT-001** MUST: All cross-cutting RPC request lifecycle concerns, including request logging, analytics tracking, and error normalization, MUST be implemented as modular @connectrpc/connect interceptors in the RPC routing layer.

### Verify

```bash
# Discover and run the project static analysis and type checking suite to verify @connectrpc/connect interceptor type signatures
# Discover and execute the automated test runner to run unit and integration tests covering the RPC interceptor pipeline
```

**Accept when:**
- Static type analysis verifies that all interceptors strictly conform to @connectrpc/connect interceptor signatures without type errors
- Automated test suites covering RPC interceptors pass successfully, verifying that logging, error handling, and context propagation execute as specified

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>