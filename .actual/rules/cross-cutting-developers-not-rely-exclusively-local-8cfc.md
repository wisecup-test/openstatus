---
version: 2
name: cross-cutting-developers-not-rely-exclusively-local-8cfc
description: "@sentry/nextjs Client Exception Capture and Error Tracing: Developers Not Rely Exclusively Local Browser"
metadata:
  actual:
    adr_id: 8cfc8f99-708d-4b05-b43c-3982a5cce735
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

# @sentry/nextjs Client Exception Capture and Error Tracing: Developers Not Rely Exclusively Local Browser

Scope: applies to `apps/dashboard/src/app/**`, `apps/dashboard/src/lib/trpc/**`.

These rules are ALWAYS ACTIVE for all client-side user interface error boundary components and shared client-side RPC and API communication error handlers.

### Rules

- **R-SENTRY-001** MUST_NOT: Developers MUST NOT rely exclusively on local browser console logging for reporting unhandled production runtime exceptions.

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
Claude Code MUST NOT skip or defer verification. All changes must be verified via project linting, automated test suites, and peer review. Violations will block pull requests.
</enforcement>