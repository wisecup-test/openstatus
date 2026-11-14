---
version: 2
name: cross-cutting-shared-client-side-rpc-error-56f6
description: "@sentry/nextjs Client Exception Capture and Error Tracing: Shared Client Side Rpc Error Handlers"
metadata:
  actual:
    adr_id: 56f6a6bf-4f1b-4ff7-acc5-ffd4448f8125
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

# @sentry/nextjs Client Exception Capture and Error Tracing: Shared Client Side Rpc Error Handlers

Scope: applies to `apps/dashboard/src/app/**`, `apps/dashboard/src/lib/trpc/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SENTRY-001** MUST: Shared client-side RPC error handlers MUST pass contextual invocation path information inside the extra metadata parameter when invoking Sentry.captureException.

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
Claude Code MUST NOT skip or defer verification. All pull requests missing Sentry error capture in error boundaries or client RPC handlers are blocked from merge.
</enforcement>