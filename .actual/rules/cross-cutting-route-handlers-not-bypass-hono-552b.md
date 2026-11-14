---
version: 2
name: cross-cutting-route-handlers-not-bypass-hono-552b
description: "Hono Routing and Context Propagation for HTTP Service Boundaries: Route Handlers Not Bypass Hono Context"
metadata:
  actual:
    adr_id: 552bfb3f-32a7-4d27-8cae-ea35672a9ecc
    status: accepted
    candidate_group_id: cd159887-e959-480c-aaac-a83ab1895daf
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Hono Routing and Context Propagation for HTTP Service Boundaries: Route Handlers Not Bypass Hono Context

Scope: applies to `apps/server/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HONO-001** MUST_NOT: Route handlers MUST NOT bypass Hono Context abstractions to store request-scoped data in process-global or module-scoped state.

### Verify

```bash
# Discover the test runner command from the repository configuration and execute tests
# Discover the static analysis and type checking command from the workspace manifest and execute it
```

**Accept when:**
- All HTTP endpoints successfully route requests and respond with expected status codes and payloads through the framework pipeline.
- Static type analysis passes with zero type errors for context setter and getter invocations across route handlers and middleware.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>