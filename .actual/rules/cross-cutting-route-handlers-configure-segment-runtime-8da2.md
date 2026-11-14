---
version: 2
name: cross-cutting-route-handlers-configure-segment-runtime-8da2
description: "next/server Route Handler Adoption for Scheduled Checker Endpoints: Route Handlers Configure Segment Runtime Properties"
metadata:
  actual:
    adr_id: 8da28512-218d-4d9c-a936-6bcbcbb848db
    status: accepted
    candidate_group_id: f2ddf12e-7bcf-45e4-a2e3-65606ce03474
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/checker/cron/**
---

# next/server Route Handler Adoption for Scheduled Checker Endpoints: Route Handlers Configure Segment Runtime Properties

Scope: applies to `apps/web/src/app/api/checker/cron/**`.

These rules are ALWAYS ACTIVE for HTTP route handlers designated for recurring checker tasks, scheduled cron intervals, and server-side endpoint handlers coordinating periodic background evaluation.

### Rules

- **R-ADR-001** MAY: Route handlers MAY configure route segment runtime properties supported by next/server when specific execution duration or compute constraints require customization.
- **R-ADR-002** MANDATORY: Discover and adhere to tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-ADR-003** MANDATORY: Execute lock-version grounding before writing code that uses a versioned library by inspecting manifest, build tool, lock file, and fetching official version-specific documentation.

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
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated pull request continuous integration checks and architectural code review.
</enforcement>