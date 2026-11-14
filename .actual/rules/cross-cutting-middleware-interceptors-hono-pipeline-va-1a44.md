---
version: 2
name: cross-cutting-middleware-interceptors-hono-pipeline-va-1a44
description: "Hono HTTP Routing and Context State Pipeline: Middleware Interceptors Hono Pipeline Validate Required"
metadata:
  actual:
    adr_id: 1a44b50f-8c02-45f4-9b02-ef06cb533473
    status: accepted
    candidate_group_id: 9480bf4e-9d57-49c4-9ad4-1389d4af2ffe
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/slack/**
---

# Hono HTTP Routing and Context State Pipeline: Middleware Interceptors Hono Pipeline Validate Required

Scope: applies to `apps/server/src/routes/slack/**`.

These rules are ALWAYS ACTIVE for HTTP route definitions and middleware pipelines handling server-side ingress traffic, and per-request state propagation between upstream verification middleware and downstream handlers.

### Rules

- **R-HONO-001** MUST: Middleware interceptors in the Hono pipeline validate required service configuration and fail fast with an appropriate HTTP error response via context response methods before invoking downstream handlers with `next()`.

### Verify

```bash
# Discover and run the project routing and middleware unit test suite.
# Discover and run the project type checking script to verify context key type safety.
# Discover and run the project linting suite to ensure middleware standards are upheld.
```

**Accept when:**
- All route integration tests pass with simulated valid and invalid configuration states.
- Static type analysis passes with zero type errors across all route context access calls.
- Middleware correctly halts unconfigured requests with error status responses before invoking downstream handlers.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration test passes, automated static type check validation, and peer code review enforce these rules.
</enforcement>