---
version: 2
name: cross-cutting-boundary-authorization-middleware-valida-d468
description: "Hono Middleware Pipeline for Boundary Authorization and Context Telemetry: Boundary Authorization Middleware Validate Incoming Request"
metadata:
  actual:
    adr_id: d46807db-977d-4417-8629-9ff65360f1fe
    status: accepted
    candidate_group_id: 20d6c0b5-4c6c-4f44-8245-265a2917445e
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/**
---

# Hono Middleware Pipeline for Boundary Authorization and Context Telemetry: Boundary Authorization Middleware Validate Incoming Request

Scope: applies to `apps/workflows/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-BND-001** MUST: Boundary authorization middleware MUST validate incoming request authorization headers against configured environment secrets and terminate unauthorized requests with a 401 status response without delegating to downstream handlers.

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