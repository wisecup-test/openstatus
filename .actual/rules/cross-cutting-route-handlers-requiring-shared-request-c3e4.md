---
version: 2
name: cross-cutting-route-handlers-requiring-shared-request-c3e4
description: "Hono Routing and Context Propagation for HTTP Service Boundaries: Route Handlers Requiring Shared Request Scoped"
metadata:
  actual:
    adr_id: c3e4b9d2-c348-4e61-bb1b-760da9888418
    status: accepted
    candidate_group_id: cd159887-e959-480c-aaac-a83ab1895daf
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Hono Routing and Context Propagation for HTTP Service Boundaries: Route Handlers Requiring Shared Request Scoped

Scope: applies to `apps/server/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HONO-001** MUST: Route handlers requiring shared request-scoped state MUST retrieve injected dependencies and metadata exclusively via the Hono Context get method.

### Verify

```bash
# Discover and run test suite from repository configuration
# Discover and run static analysis / type checking from workspace manifest
```

**Accept when:**
- All HTTP endpoints successfully route requests and respond with expected status codes and payloads through the framework pipeline.
- Static type analysis passes with zero type errors for context setter and getter invocations across route handlers and middleware.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>