---
version: 2
name: cross-cutting-handlers-not-mutate-shared-process-4884
description: "Hono HTTP Routing and Context State Pipeline: Handlers Not Mutate Shared Process Level"
metadata:
  actual:
    adr_id: 4884cc11-dffe-4209-ae1e-cd5bf7017759
    status: accepted
    candidate_group_id: 9480bf4e-9d57-49c4-9ad4-1389d4af2ffe
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/slack/**
---

# Hono HTTP Routing and Context State Pipeline: Handlers Not Mutate Shared Process Level

Scope: applies to `apps/server/src/routes/slack/**`.

These rules are ALWAYS ACTIVE for all HTTP route definitions, middleware pipelines, and per-request state propagation modules.

### Rules

- **R-HONO-001** MUST_NOT: Handlers MUST NOT mutate shared process-level state or persist per-request data outside the active Hono context object across asynchronous execution boundaries.

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
Claude Code MUST NOT skip or defer verification.
</enforcement>