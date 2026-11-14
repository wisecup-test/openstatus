---
version: 2
name: cross-cutting-scheduled-next-server-route-handlers-e142
description: "next/server Route Handler Adoption for Scheduled Checker Endpoints: Scheduled Next Server Route Handlers Not"
metadata:
  actual:
    adr_id: e1427cee-fdc1-4172-8068-563a4062b684
    status: accepted
    candidate_group_id: f2ddf12e-7bcf-45e4-a2e3-65606ce03474
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/checker/cron/**
---

# next/server Route Handler Adoption for Scheduled Checker Endpoints: Scheduled Next Server Route Handlers Not

Scope: applies to `apps/web/src/app/api/checker/cron/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SCHED-001** MUST_NOT: Scheduled next/server route handlers MUST NOT embed cadence execution loops or business evaluation routines directly within the HTTP handler boundary.

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