---
version: 2
name: cross-cutting-applications-route-uncaught-runtime-exce-1923
description: "@sentry/nextjs Exception Capture and Boundary Tracing: Applications Route Uncaught Runtime Exceptions Failed"
metadata:
  actual:
    adr_id: 19238276-5008-4121-921b-d2ffdfc2ce25
    status: accepted
    candidate_group_id: a868f887-5323-4bac-b14e-89765e49358b
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
      - src/lib/trpc/**
---

# @sentry/nextjs Exception Capture and Boundary Tracing: Applications Route Uncaught Runtime Exceptions Failed

Scope: applies to `apps/status-page/src/app/**`, `apps/status-page/src/lib/trpc/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SENTRY-001** MUST: Applications MUST route uncaught runtime exceptions and failed remote procedure call responses to @sentry/nextjs using Sentry.captureException at global boundary handlers.

### Verify

```bash
# Discover the repository script runner and execute static analysis and test suite targets for boundary tracing compliance
npm test
```

**Accept when:**
- Static analysis confirms that unhandled boundary error handlers invoke Sentry.captureException.
- Automated verification passes with contextual path metadata validated in remote call error interceptors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>