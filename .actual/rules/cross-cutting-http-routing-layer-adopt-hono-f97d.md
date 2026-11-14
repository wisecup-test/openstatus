---
version: 2
name: cross-cutting-http-routing-layer-adopt-hono-f97d
description: "Hono HTTP Routing and Context State Pipeline: Http Routing Layer Adopt Hono Route"
metadata:
  actual:
    adr_id: f97dfe74-ea88-4552-82da-93c7cea79c4a
    status: accepted
    candidate_group_id: 9480bf4e-9d57-49c4-9ad4-1389d4af2ffe
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/slack/**
---

# Hono HTTP Routing and Context State Pipeline: Http Routing Layer Adopt Hono Route

Scope: applies to `apps/server/src/routes/slack/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HONO-001** MUST: The HTTP routing layer MUST adopt Hono for route definition and middleware composition, utilizing the Hono context pipeline (`c.set` and `c.get`) to propagate request-scoped configuration and parsed payloads between middleware and downstream handlers.

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
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>