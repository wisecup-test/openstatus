---
version: 2
name: cross-cutting-cross-cutting-middleware-propagate-reque-63ae
description: "Hono Routing and Context Propagation for HTTP Service Boundaries: Cross Cutting Middleware Propagate Request Scoped"
metadata:
  actual:
    adr_id: 63ae9646-d564-43a8-8e70-ec3809186260
    status: accepted
    candidate_group_id: cd159887-e959-480c-aaac-a83ab1895daf
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Hono Routing and Context Propagation for HTTP Service Boundaries: Cross Cutting Middleware Propagate Request Scoped

Scope: applies to `apps/server/src/**`.

These rules are ALWAYS ACTIVE for all HTTP service endpoints, route handlers, and middleware definitions exposed by the server application.

### Rules

- **R-HONO-001** MUST: Cross-cutting middleware MUST propagate request-scoped metadata using the Hono Context set method.

### Verify

```bash
# Discover and execute the test suite from the repository configuration
# Discover and execute the static analysis / type checking command from the workspace manifest
```

**Accept when:**
- All HTTP endpoints successfully route requests and respond with expected status codes and payloads through the framework pipeline.
- Static type analysis passes with zero type errors for context setter and getter invocations across route handlers and middleware.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>