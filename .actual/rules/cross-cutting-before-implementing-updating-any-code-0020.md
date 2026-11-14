---
version: 2
name: cross-cutting-before-implementing-updating-any-code-0020
description: "Hono HTTP Routing and Context State Pipeline: Before Implementing Updating Any Code Depending"
metadata:
  actual:
    adr_id: 0020aed7-65f2-489b-b049-10668cace28a
    status: accepted
    candidate_group_id: 9480bf4e-9d57-49c4-9ad4-1389d4af2ffe
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/slack/**
---

# Hono HTTP Routing and Context State Pipeline: Before Implementing Updating Any Code Depending

Scope: applies to `apps/server/src/routes/slack/**`.

These rules are ALWAYS ACTIVE for all HTTP route definitions, middleware pipelines handling server-side ingress traffic, and per-request state propagation between upstream verification middleware and downstream handlers.

### Rules

- **R-HONO-001** MUST: Before implementing or updating any code depending on Hono or its submodules, the consumer MUST inspect the repository lock artifact to determine the exact resolved version and verify that all referenced APIs exist in that version's documentation.

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
Claude Code MUST NOT skip or defer verification. Continuous integration test passes, automated static type checks, and peer code reviews are mandatory.
</enforcement>