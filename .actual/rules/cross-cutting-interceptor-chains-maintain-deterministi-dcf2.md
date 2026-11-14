---
version: 2
name: cross-cutting-interceptor-chains-maintain-deterministi-dcf2
description: "@connectrpc/connect Interceptor Pipeline Architecture: Interceptor Chains Maintain Deterministic Execution Ordering"
metadata:
  actual:
    adr_id: dcf2007b-52ea-448c-bf81-40f148999a9b
    status: accepted
    candidate_group_id: 4d15d7b7-58ac-4c28-ae26-f4b6181018d4
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/rpc/interceptors/**
---

# @connectrpc/connect Interceptor Pipeline Architecture: Interceptor Chains Maintain Deterministic Execution Ordering

Scope: applies to `apps/server/src/routes/rpc/interceptors/**`.

These rules are ALWAYS ACTIVE for all inbound and outbound RPC service procedures handled through the @connectrpc/connect server routing layer and cross-cutting request lifecycle handlers.

### Rules

- **R-CONN-001** SHOULD: Interceptor chains SHOULD maintain deterministic execution ordering by placing contextual authentication and identification interceptors ahead of logging, error handling, and analytics interceptors.

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