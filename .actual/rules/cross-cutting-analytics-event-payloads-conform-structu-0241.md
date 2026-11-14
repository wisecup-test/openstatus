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

These rules are ALWAYS ACTIVE for all server-side REST route handlers, RPC interceptors, and HTTP middlewares that process incoming requests or perform domain mutations.

### Rules

- **R-ANALYTICS-001** MUST: Analytics event payloads MUST conform to the structured event definitions maintained by @openstatus/analytics.
- **R-ANALYTICS-002** MUST: Extract the active workspace identifier from the request context before invoking analytics dispatch routines.
- **R-ANALYTICS-003** MUST: Catch analytics delivery errors and log them through the structured logging facility at warning level with event and workspace context attributes.

### Verify

```bash
# Discover and execute workspace static analysis and type check scripts from the repository manifests
npm run typecheck

# Discover and run the test suite scripts for server routes and interceptors
npm test
```

**Accept when:**
- All server routes, interceptors, and middlewares compile without type errors against the @openstatus/analytics interface.
- Analytics event dispatches correctly associate workspace identity from request context and handle transmission failures gracefully without unhandled exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>