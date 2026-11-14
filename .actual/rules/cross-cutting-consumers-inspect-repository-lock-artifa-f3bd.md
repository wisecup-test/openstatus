---
version: 2
name: cross-cutting-consumers-inspect-repository-lock-artifa-f3bd
description: "@connectrpc/connect Interceptor Pipeline Architecture: Consumers Inspect Repository Lock Artifact Verify"
metadata:
  actual:
    adr_id: f3bda3cb-18de-4756-99e5-7ef9a0c43c29
    status: accepted
    candidate_group_id: 4d15d7b7-58ac-4c28-ae26-f4b6181018d4
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/rpc/interceptors/**
---

# @connectrpc/connect Interceptor Pipeline Architecture: Consumers Inspect Repository Lock Artifact Verify

Scope: applies to `apps/server/src/routes/rpc/interceptors/**`.

These rules are ALWAYS ACTIVE for all inbound and outbound RPC service procedures handled through the @connectrpc/connect server routing layer and cross-cutting request lifecycle handlers.

### Rules

- **R-CONNECT-001** MUST: Consumers MUST inspect the repository lock artifact to verify the exact resolved version of @connectrpc/connect and validate that all referenced interceptor APIs and context handling signatures conform to the locked version documentation before implementation.

### Verify

```bash
# Discover and run the project static analysis and type checking suite to verify @connectrpc/connect interceptor type signatures
# Discover and execute the automated test runner to run unit and integration tests covering the RPC interceptor pipeline
```

**Accept when:**
- Static type analysis verifies that all interceptors strictly conform to @connectrpc/connect interceptor signatures without type errors
- Automated test suites covering RPC interceptors pass successfully, verifying that logging, error handling, and context propagation execute as specified

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory and enforced via automated CI checks and architectural peer review.
</enforcement>