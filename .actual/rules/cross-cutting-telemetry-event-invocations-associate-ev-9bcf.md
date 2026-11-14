---
version: 2
name: cross-cutting-telemetry-event-invocations-associate-ev-9bcf
description: "Adoption of @openstatus/analytics for Server Telemetry and Event Tracking: Telemetry Event Invocations Associate Events Authenticated"
metadata:
  actual:
    adr_id: 9bcf24f3-e71e-46f3-bc6e-e94aba557ca9
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

# Adoption of @openstatus/analytics for Server Telemetry and Event Tracking: Telemetry Event Invocations Associate Events Authenticated

Scope: applies to `apps/server/src/libs/middlewares/**`, `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TLM-001** MUST: Telemetry event invocations MUST associate events with the authenticated workspace identifier resolved from the request context.

### Verify

```bash
# Discover and run the type check and test scripts from the repository manifests
# Example (adjust based on discovered repository scripts):
npm run typecheck
npm run test
```

**Accept when:**
- All server routes, interceptors, and middlewares compile without type errors against the analytics interface.
- Analytics event dispatches correctly associate workspace identity from request context and handle transmission failures gracefully without unhandled exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>