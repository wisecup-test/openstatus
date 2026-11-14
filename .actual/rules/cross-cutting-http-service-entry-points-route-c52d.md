---
version: 2
name: cross-cutting-http-service-entry-points-route-c52d
description: "Hono Routing and Context Propagation for HTTP Service Boundaries: Http Service Entry Points Route Definitions"
metadata:
  actual:
    adr_id: c52d328d-4aac-4bc4-9459-5fbf67305ef5
    status: accepted
    candidate_group_id: cd159887-e959-480c-aaac-a83ab1895daf
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Hono Routing and Context Propagation for HTTP Service Boundaries: Http Service Entry Points Route Definitions

Scope: applies to `apps/server/src/**`.

These rules are ALWAYS ACTIVE for all HTTP service endpoints, route handlers, and middleware definitions exposed by the server application.

### Rules

- **R-HONO-001** MUST: HTTP service entry points and route definitions MUST be implemented using Hono application routers and middleware pipelines.

### Verify

```bash
# Discover the test runner command from the repository configuration and execute tests
# Discover the static analysis and type checking command from the workspace manifest and execute it
```

**Accept when:**
- All HTTP endpoints successfully route requests and respond with expected status codes and payloads through the framework pipeline.
- Static type analysis passes with zero type errors for context setter and getter invocations across route handlers and middleware.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration pipelines and peer code reviews validate static types, endpoint test suites, and framework context conventions.
</enforcement>