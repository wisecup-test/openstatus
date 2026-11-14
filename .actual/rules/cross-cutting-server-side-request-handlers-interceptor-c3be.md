---
version: 2
name: cross-cutting-server-side-request-handlers-interceptor-c3be
description: "Adoption of @openstatus/analytics for Server Telemetry and Event Tracking: Server Side Request Handlers Interceptors Tracking"
metadata:
  actual:
    adr_id: c3be4554-758c-4b93-a0ab-508e45400d1a
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

# Adoption of @openstatus/analytics for Server Telemetry and Event Tracking: Server Side Request Handlers Interceptors Tracking

Scope: applies to `apps/server/src/libs/middlewares/**`, `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for all server-side REST route handlers, RPC interceptors, and HTTP middlewares that process incoming requests or perform domain mutations.

### Rules

- **R-ADR-001** MUST: Server-side request handlers, interceptors, and tracking middlewares MUST emit domain and operational telemetry exclusively through @openstatus/analytics.

### Verify

```bash
# Discover and execute workspace static analysis and type check scripts
# Discover and run test suite scripts for server routes and interceptors
```

**Accept when:**
- All server routes, interceptors, and middlewares compile without type errors against the @openstatus/analytics interface.
- Analytics event dispatches correctly associate workspace identity from request context and handle transmission failures gracefully without unhandled exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration checks executing workspace type validation and test suites, and peer code reviews.
</enforcement>