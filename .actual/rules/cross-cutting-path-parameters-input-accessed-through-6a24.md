---
version: 2
name: cross-cutting-path-parameters-input-accessed-through-6a24
description: "Hono Routing and Context Propagation for HTTP Service Boundaries: Path Parameters Input Accessed Through Hono"
metadata:
  actual:
    adr_id: 6a24ec6f-7c76-4ce1-93f1-ca71f95d3699
    status: accepted
    candidate_group_id: cd159887-e959-480c-aaac-a83ab1895daf
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Hono Routing and Context Propagation for HTTP Service Boundaries: Path Parameters Input Accessed Through Hono

Scope: applies to `apps/server/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HONO-001** SHOULD: Path parameters and input parameters MUST be accessed through the Hono Context request param accessor prior to domain processing.

### Verify

```bash
# Discover and run the test suite to validate HTTP route definitions and status codes
# Discover and run static analysis / type checking to ensure Hono Context variables adhere to expected type declarations
```

**Accept when:**
- All HTTP endpoints successfully route requests and respond with expected status codes and payloads through the framework pipeline.
- Static type analysis passes with zero type errors for context setter and getter invocations across route handlers and middleware.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>