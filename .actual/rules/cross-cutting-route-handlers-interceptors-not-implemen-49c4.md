---
version: 2
name: cross-cutting-route-handlers-interceptors-not-implemen-49c4
description: "Adoption of @openstatus/analytics for Server Telemetry and Event Tracking: Route Handlers Interceptors Not Implement Independent"
metadata:
  actual:
    adr_id: 49c47acb-e849-4c96-978e-98a2bb9547fa
    status: accepted
    candidate_group_id: cf93de28-0c1e-48d4-8854-47c3358b8b7e
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/libs/middlewares/**
      - src/routes/**
---

# Adoption of @openstatus/analytics for Server Telemetry and Event Tracking: Route Handlers Interceptors Not Implement Independent

Scope: applies to `apps/server/src/libs/middlewares/**`, `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TELEMETRY-001** MUST_NOT: Route handlers and interceptors MUST NOT implement independent telemetry transport logic or bypass @openstatus/analytics with direct external metric or event endpoints.

### Verify

```bash
# Discover the workspace static analysis and type check scripts from the repository manifests and execute them across the server application.
# Discover and run the test suite scripts for server routes and interceptors to verify analytics event dispatch handling and error isolation.
```

**Accept when:**
- All server routes, interceptors, and middlewares compile without type errors against the @openstatus/analytics interface.
- Analytics event dispatches correctly associate workspace identity from request context and handle transmission failures gracefully without unhandled exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification. All server route handlers, RPC interceptors, and HTTP middlewares must be verified against these telemetry rules.
</enforcement>