---
version: 2
name: cross-cutting-route-handlers-return-standard-http-77d1
description: "next/server Route Handler Adoption for Scheduled Checker Endpoints: Route Handlers Return Standard Http Response"
metadata:
  actual:
    adr_id: 77d1bef9-b34e-48fe-8615-be713ef894cb
    status: accepted
    candidate_group_id: f2ddf12e-7bcf-45e4-a2e3-65606ce03474
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/checker/cron/**
---

# next/server Route Handler Adoption for Scheduled Checker Endpoints: Route Handlers Return Standard Http Response

Scope: applies to `apps/web/src/app/api/checker/cron/**`.

These rules are ALWAYS ACTIVE for HTTP route handlers designated for recurring checker tasks, scheduled cron intervals, and server-side endpoint handlers coordinating periodic background evaluation.

### Rules

- **R-ROUTE-001** SHOULD: Route handlers SHOULD return standard HTTP response objects provided by next/server to communicate execution status to calling schedulers.

### Verify

```bash
# Discover and execute the project static analysis and typecheck suites
# Discover and run the project test suite to validate route dispatch and error handling behavior
```

**Accept when:**
- All cadence-specific route handlers compile without type errors against the locked server module interfaces.
- Each scheduled route handler correctly delegates execution to the internal cron runner module and reports unhandled exceptions.
- Project verification test suites pass for all scheduled endpoint routes.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>