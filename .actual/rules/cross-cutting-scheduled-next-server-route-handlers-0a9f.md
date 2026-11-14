---
version: 2
name: cross-cutting-scheduled-next-server-route-handlers-0a9f
description: "next/server Route Handler Adoption for Scheduled Checker Endpoints: Scheduled Next Server Route Handlers Coordinate"
metadata:
  actual:
    adr_id: 0a9f489f-0d77-4655-8f92-a12a89ee68ea
    status: accepted
    candidate_group_id: f2ddf12e-7bcf-45e4-a2e3-65606ce03474
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/checker/cron/**
---

# next/server Route Handler Adoption for Scheduled Checker Endpoints: Scheduled Next Server Route Handlers Coordinate

Scope: applies to `apps/web/src/app/api/checker/cron/**`.

These rules are ALWAYS ACTIVE for HTTP route handlers designated for recurring checker tasks, scheduled cron intervals, and server-side endpoint handlers coordinating periodic background evaluation.

### Rules

- **R-SCHED-001** MUST: Scheduled next/server route handlers MUST coordinate execution monitoring and error reporting through the shared _sentry module.

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
Claude Code MUST NOT skip or defer verification.
</enforcement>