---
version: 2
name: cross-cutting-individual-rpc-service-procedure-impleme-2290
description: "@connectrpc/connect Interceptor Pipeline Architecture: Individual Rpc Service Procedure Implementations Not"
metadata:
  actual:
    adr_id: 22909080-38a5-427e-9bf7-0a41715ab86a
    status: accepted
    candidate_group_id: 4d15d7b7-58ac-4c28-ae26-f4b6181018d4
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/rpc/interceptors/**
---

# @connectrpc/connect Interceptor Pipeline Architecture: Individual Rpc Service Procedure Implementations Not

Scope: applies to `apps/server/src/routes/rpc/interceptors/**`.

These rules are ALWAYS ACTIVE for all inbound and outbound RPC service procedures handled through the @connectrpc/connect server routing layer and cross-cutting request lifecycle handlers.

### Rules

- **R-RPC-001** MUST_NOT: Individual RPC service procedure implementations MUST NOT directly execute transport logging, global exception formatting, or ambient telemetry dispatch that belongs within interceptor chains.

### Verify

```bash
# Discover and run the project static analysis and type checking suite to verify @connectrpc/connect interceptor type signatures
# Discover and execute the automated test runner to run unit and integration tests covering the RPC interceptor pipeline
```

**Accept when:**
- Static type analysis verifies that all interceptors strictly conform to @connectrpc/connect interceptor signatures without type errors
- Automated test suites covering RPC interceptors pass successfully, verifying that logging, error handling, and context propagation execute as specified

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration checks and mandatory architectural peer reviews.
</enforcement>