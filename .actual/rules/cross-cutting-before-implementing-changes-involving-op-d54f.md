---
version: 2
name: cross-cutting-before-implementing-changes-involving-op-d54f
description: "Adoption of @openstatus/analytics for Server Telemetry and Event Tracking: Before Implementing Changes Involving Openstatus Analytics"
metadata:
  actual:
    adr_id: d54f0779-6fd3-4fd2-9a9a-2cbd0d6698a4
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

# Adoption of @openstatus/analytics for Server Telemetry and Event Tracking: Before Implementing Changes Involving Openstatus Analytics

Scope: applies to `apps/server/src/libs/middlewares/**`, `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OPENSTATUS-001** MUST: Before implementing changes involving @openstatus/analytics or related dependencies, developers MUST discover the dependency lock artifact in the project repository and verify the exact resolved version.

### Verify

```bash
# Discover workspace static analysis and type check scripts from the repository manifests and execute them across the server application.
# Discover and run test suite scripts for server routes and interceptors to verify analytics event dispatch handling and error isolation.
```

**Accept when:**
- All server routes, interceptors, and middlewares compile without type errors against the @openstatus/analytics interface.
- Analytics event dispatches correctly associate workspace identity from request context and handle transmission failures gracefully without unhandled exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>