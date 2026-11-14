---
version: 2
name: cross-cutting-middleware-interceptors-calculate-execut-375e
description: "Hono Middleware Pipeline for Boundary Authorization and Context Telemetry: Middleware Interceptors Calculate Execution Duration Record"
metadata:
  actual:
    adr_id: 375e0811-d624-4d4d-898b-ed1a1184e3b6
    status: accepted
    candidate_group_id: 20d6c0b5-4c6c-4f44-8245-265a2917445e
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/**
---

# Hono Middleware Pipeline for Boundary Authorization and Context Telemetry: Middleware Interceptors Calculate Execution Duration Record

Scope: applies to `apps/workflows/src/**`.

These rules are ALWAYS ACTIVE for HTTP route definitions, sub-routers, and workflow trigger endpoints exposed at service boundaries requiring pre-flight secret authentication or contextual telemetry enrichment.

### Rules

- **R-HONO-001** SHOULD: Middleware interceptors SHOULD calculate execution duration and record canonical status and telemetry outcomes following downstream handler completion.

### Verify

```bash
# Discover and run the project static analysis and linting scripts to verify middleware registration and rule compliance across route entry points.
# Discover and run the project automated test suite to verify that boundary middleware correctly rejects unauthorized requests and populates telemetry context.
```

**Accept when:**
- All service endpoint test suites pass with middleware intercepting and rejecting unauthorized requests with a 401 status.
- Static verification confirms that perimeter authentication and request context tracking are declared via route middleware bindings rather than duplicated inside individual endpoint handlers.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>