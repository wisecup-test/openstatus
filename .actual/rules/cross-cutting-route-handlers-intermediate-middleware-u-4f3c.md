---
version: 2
name: cross-cutting-route-handlers-intermediate-middleware-u-4f3c
description: "Hono HTTP Routing and Context State Pipeline: Route Handlers Intermediate Middleware Use Hono"
metadata:
  actual:
    adr_id: 4f3ca016-a2fb-4288-9ce7-63767fd9aa06
    status: accepted
    candidate_group_id: 9480bf4e-9d57-49c4-9ad4-1389d4af2ffe
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/slack/**
---

# Hono HTTP Routing and Context State Pipeline: Route Handlers Intermediate Middleware Use Hono

Scope: applies to `apps/server/src/routes/slack/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HONO-001** SHOULD: Route handlers and intermediate middleware SHOULD use `hono/factory` utilities to construct modular, reusable middleware functions rather than inline unbounded handlers.

### Verify

```bash
# Discover and run the project routing and middleware unit test suite
# Discover and run the project type checking script to verify context key type safety
# Discover and run the project linting suite to ensure middleware standards are upheld
```

**Accept when:**
- All route integration tests pass with simulated valid and invalid configuration states.
- Static type analysis passes with zero type errors across all route context access calls.
- Middleware correctly halts unconfigured requests with error status responses before invoking downstream handlers.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>