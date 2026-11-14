---
version: 2
name: cross-cutting-route-definitions-sub-routers-exposed-6fdd
description: "Hono Middleware Pipeline for Boundary Authorization and Context Telemetry: Route Definitions Sub Routers Exposed Across"
metadata:
  actual:
    adr_id: 6fddf9c4-5c5e-46da-be85-85d76390213d
    status: accepted
    candidate_group_id: 20d6c0b5-4c6c-4f44-8245-265a2917445e
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/**
---

# Hono Middleware Pipeline for Boundary Authorization and Context Telemetry: Route Definitions Sub Routers Exposed Across

Scope: applies to `apps/workflows/src/**`.

These rules are ALWAYS ACTIVE for HTTP route definitions, sub-routers, and workflow trigger endpoints exposed at service boundaries requiring pre-flight secret authentication or contextual telemetry enrichment.

### Rules

- **R-HONO-001** MUST: All route definitions and sub-routers exposed across service boundaries MUST register boundary authorization and context telemetry handlers using Hono middleware interception prior to dispatching requests to endpoint business logic.
- **R-HONO-002** MUST: Middleware registration order MUST ensure that correlation identifiers and telemetry context wrappers precede authorization filters, ensuring rejected requests generate observable audit trails.
- **R-HONO-003** MUST: Context variables such as request identifiers and wide event objects MUST be propagated using framework execution context storage rather than global state.

### Verify

```bash
# Discover and run the project static analysis and linting scripts to verify middleware registration and rule compliance across route entry points.
# Discover and run the project automated test suite to verify that boundary middleware correctly rejects unauthorized requests and populates telemetry context.
```

**Accept when:**
- All service endpoint test suites pass with middleware intercepting and rejecting unauthorized requests with a 401 status.
- Static verification confirms that perimeter authentication and request context tracking are declared via route middleware bindings rather than duplicated inside individual endpoint handlers.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated integration tests, contract tests, and static analysis.
</enforcement>