---
version: 2
name: cross-cutting-route-definitions-declare-http-verbs-8aac
description: "Hono Routing and Context Propagation for HTTP Service Boundaries: Route Definitions Declare Http Verbs Through"
metadata:
  actual:
    adr_id: 8aac96af-ca9a-4742-a4ed-8ce83124e6dc
    status: accepted
    candidate_group_id: cd159887-e959-480c-aaac-a83ab1895daf
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Hono Routing and Context Propagation for HTTP Service Boundaries: Route Definitions Declare Http Verbs Through

Scope: applies to `apps/server/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HONO-001** MUST: Route definitions MUST declare HTTP verbs through router method invocations and return standardized JSON responses using the Hono Context json helper.

### Verify

```bash
# Discover the test runner command from the repository configuration and execute the test suite
# Discover the static analysis and type checking command from the workspace manifest and execute it
```

**Accept when:**
- All HTTP endpoints successfully route requests and respond with expected status codes and payloads through the framework pipeline.
- Static type analysis passes with zero type errors for context setter and getter invocations across route handlers and middleware.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>