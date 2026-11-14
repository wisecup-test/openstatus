---
version: 2
name: cross-cutting-endpoint-route-handlers-not-duplicate-1354
description: "Hono Middleware Pipeline for Boundary Authorization and Context Telemetry: Endpoint Route Handlers Not Duplicate Perimeter"
metadata:
  actual:
    adr_id: 1354a3b4-4e25-4436-a804-a5f31b83c155
    status: accepted
    candidate_group_id: 20d6c0b5-4c6c-4f44-8245-265a2917445e
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/**
---

# Hono Middleware Pipeline for Boundary Authorization and Context Telemetry: Endpoint Route Handlers Not Duplicate Perimeter

Scope: applies to `apps/workflows/src/**`.

These rules are ALWAYS ACTIVE for HTTP route definitions, sub-routers, workflow trigger endpoints, and boundary endpoints requiring pre-flight secret authentication or contextual telemetry enrichment.

### Rules

- **R-HONO-001** MUST_NOT: Endpoint route handlers MUST NOT duplicate perimeter authorization header checks or ad-hoc correlation identifier generation managed by the boundary middleware pipeline.

### Verify

```bash
# Discover and run the project static analysis and linting scripts to verify middleware registration and rule compliance across route entry points.
# Discover and run the project automated test suite to verify that boundary middleware correctly rejects unauthorized requests and populates telemetry context.
```

**Accept when:**
- All service endpoint test suites pass with middleware intercepting and rejecting unauthorized requests with a 401 status.
- Static verification confirms that perimeter authentication and request context tracking are declared via route middleware bindings rather than duplicated inside individual endpoint handlers.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>