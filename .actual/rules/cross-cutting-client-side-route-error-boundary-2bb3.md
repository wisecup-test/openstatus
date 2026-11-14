---
version: 2
name: cross-cutting-client-side-route-error-boundary-2bb3
description: "@sentry/nextjs Client Exception Capture and Error Tracing: Client Side Route Error Boundary Components"
metadata:
  actual:
    adr_id: 2bb3fd2d-317b-42bf-9f08-7b74faaad108
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

# @sentry/nextjs Client Exception Capture and Error Tracing: Client Side Route Error Boundary Components

Scope: applies to `apps/dashboard/src/app/**`, `apps/dashboard/src/lib/trpc/**`.

These rules are ALWAYS ACTIVE for all client-side user interface error boundary components and shared client-side RPC and API communication error handlers.

### Rules

- **R-SENTRY-001** MUST: All client-side route error boundary components MUST invoke Sentry.captureException within client lifecycle hooks upon receiving an unhandled error.

### Verify

```bash
# Discover and execute project linting, static analysis, and automated test suite scripts
# to verify adherence to error logging standards and validate error boundary reporting behavior.
```

**Accept when:**
- All client-side error boundary components invoke Sentry.captureException within error lifecycle hooks.
- Client-side RPC error dispatchers invoke Sentry.captureException with associated route extra metadata before propagation or warning.
- Static analysis checks and test suites confirm that unhandled client exceptions report to Sentry without missing contextual payloads.

<enforcement>
Claude Code MUST NOT skip or defer verification. All modifications to shared communication layers and error boundary components require automated verification and peer review.
</enforcement>