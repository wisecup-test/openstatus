---
version: 2
name: cross-cutting-failures-encountered-during-analytics-ev-b6f4
description: "Adoption of @openstatus/analytics for Server Telemetry and Event Tracking: Failures Encountered During Analytics Event Emission"
metadata:
  actual:
    adr_id: b6f4cba3-1895-4b0b-ae19-6a3e16f92e17
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

# Adoption of @openstatus/analytics for Server Telemetry and Event Tracking: Failures Encountered During Analytics Event Emission

Scope: applies to `apps/server/src/libs/middlewares/**`, `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for all server-side REST route handlers, RPC interceptors, and HTTP middlewares that process incoming requests or perform domain mutations.

### Rules

- **R-ANALYTICS-001** SHOULD: Failures encountered during analytics event emission SHOULD be caught and logged as non-fatal warnings to prevent telemetry disruptions from failing primary request transactions.
- **R-ANALYTICS-002** MANDATORY: Extract the active workspace identifier from the request context before invoking analytics dispatch routines.
- **R-ANALYTICS-003** MANDATORY: Catch analytics delivery errors and log them through the structured logging facility at warning level with event and workspace context attributes.
- **R-ANALYTICS-004** MANDATORY: Enforce workspace-level type checking and contract compatibility testing in CI pipelines prior to merging dependency updates.

### Verify

```bash
# Discover and execute workspace static analysis and type check scripts across the server application
# Discover and run test suite scripts for server routes and interceptors
```

**Accept when:**
- All server routes, interceptors, and middlewares compile without type errors against the @openstatus/analytics interface.
- Analytics event dispatches correctly associate workspace identity from request context and handle transmission failures gracefully without unhandled exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks and peer code reviews enforce these rules.
</enforcement>