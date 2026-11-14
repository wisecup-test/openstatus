---
version: 2
name: cross-cutting-before-implementing-modifying-route-hand-f159
description: "next/server Route Handler Adoption for Scheduled Checker Endpoints: Before Implementing Modifying Route Handlers Next"
metadata:
  actual:
    adr_id: f159c1be-5c22-4b0a-af42-aab00741b6ed
    status: accepted
    candidate_group_id: f2ddf12e-7bcf-45e4-a2e3-65606ce03474
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/checker/cron/**
---

# next/server Route Handler Adoption for Scheduled Checker Endpoints: Before Implementing Modifying Route Handlers Next

Scope: applies to `apps/web/src/app/api/checker/cron/**`.

These rules are ALWAYS ACTIVE for HTTP route handlers designated for recurring checker tasks, scheduled cron intervals, and server-side endpoint handlers coordinating periodic background evaluation.

### Rules

- **R-ROUTE-001** MUST: Before implementing or modifying route handlers using next/server, the consumer MUST inspect the repository lock artifact to determine the exact resolved dependency version and verify supported server API contracts against that version official documentation.
- **R-ROUTE-002** MUST: Configure each interval endpoint with a dedicated cadence path while routing execution through the common runner abstraction.
- **R-ROUTE-003** MUST: Ensure error tracking contexts capture the originating interval identifier before flushing telemetry through the monitoring module.

### Verify

```bash
# Discover and execute the project static analysis and typecheck suites to verify route handler compliance with next/server export contracts.
# Discover and run the project test suite to validate route dispatch and error handling behavior across scheduled intervals.
```

**Accept when:**
- All cadence-specific route handlers compile without type errors against the locked server module interfaces.
- Each scheduled route handler correctly delegates execution to the internal cron runner module and reports unhandled exceptions.
- Project verification test suites pass for all scheduled endpoint routes.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated pull request continuous integration checks and architectural code reviews verify compliance.
</enforcement>