---
version: 2
name: cross-cutting-developers-sanitize-error-payloads-metad-43ae
description: "@sentry/nextjs Client Exception Capture and Error Tracing: Developers Sanitize Error Payloads Metadata Properties"
metadata:
  actual:
    adr_id: 43ae944e-e83d-4cf3-8a12-92047bc40bd5
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

# @sentry/nextjs Client Exception Capture and Error Tracing: Developers Sanitize Error Payloads Metadata Properties

Scope: applies to `apps/dashboard/src/app/**`, `apps/dashboard/src/lib/trpc/**`.

These rules are ALWAYS ACTIVE for all client-side user interface error boundary components and shared client-side RPC and API communication error handlers.

### Rules

- **R-SENTRY-001** SHOULD: Developers sanitize error payloads and metadata properties to prevent leaking sensitive operational tokens or user information into telemetry traces.

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
Claude Code MUST NOT skip or defer verification. All modifications to shared communication layers and error boundary components are verified by automated continuous integration pipeline analysis and mandatory peer review.
</enforcement>