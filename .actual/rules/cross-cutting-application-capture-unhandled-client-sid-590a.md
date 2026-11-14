---
version: 2
name: cross-cutting-application-capture-unhandled-client-sid-590a
description: "@sentry/nextjs Client Exception Capture and Error Tracing: Application Capture Unhandled Client Side Runtime"
metadata:
  actual:
    adr_id: 590ac648-a1e2-4b37-98e8-bc7f4a3c02d3
    status: accepted
    candidate_group_id: 0c5cef7f-e698-4627-b0e0-54cab25681c9
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/app/**
      - src/lib/trpc/**
---

# @sentry/nextjs Client Exception Capture and Error Tracing: Application Capture Unhandled Client Side Runtime

Scope: applies to `apps/dashboard/src/app/**`, `apps/dashboard/src/lib/trpc/**`.

These rules are ALWAYS ACTIVE for all client-side user interface error boundary components and shared client-side RPC and API communication error handlers.

### Rules

- **R-SENTRY-001** MUST: The application MUST capture all unhandled client-side runtime errors and failed remote procedure calls using @sentry/nextjs Sentry.captureException for centralized exception tracing.

### Verify

```bash
# Discover and execute the project linting and static analysis scripts to verify adherence to error logging standards.
# Discover and run the project automated test suite to ensure error boundary reporting behavior is validated.
```

**Accept when:**
- All client-side error boundary components invoke Sentry.captureException within error lifecycle hooks.
- Client-side RPC error dispatchers invoke Sentry.captureException with associated route extra metadata before propagation or warning.
- Static analysis checks and test suites confirm that unhandled client exceptions report to Sentry without missing contextual payloads.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration pipeline analyzing source files and mandatory peer review.
</enforcement>