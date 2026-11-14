---
version: 2
name: cross-cutting-analytics-event-payloads-conform-structu-0241
description: "Adoption of @openstatus/analytics for Server Telemetry and Event Tracking: Analytics Event Payloads Conform Structured Definitions"
metadata:
  actual:
    adr_id: 024108d0-eb0a-4132-b8f0-c95894eeb969
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

# Adoption of @openstatus/analytics for Server Telemetry and Event Tracking: Analytics Event Payloads Conform Structured Definitions

Scope: applies to `apps/server/src/libs/middlewares/**`, `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TELEMETRY-001** MUST: Analytics event payloads MUST conform to the structured event definitions maintained by @openstatus/analytics.

### Verify

```bash
# Discover the workspace static analysis and type check scripts from the repository manifests and execute them
npm run typecheck
# Run the test suite scripts for server routes and interceptors
npm test
```

**Accept when:**
- All server routes, interceptors, and middlewares compile without type errors against the @openstatus/analytics interface.
- Analytics event dispatches correctly associate workspace identity from request context and handle transmission failures gracefully without unhandled exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>