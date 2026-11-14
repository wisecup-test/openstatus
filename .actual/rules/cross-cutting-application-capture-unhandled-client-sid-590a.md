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

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CLI-001** MUST: The application MUST capture all unhandled client-side runtime errors and failed remote procedure calls using @sentry/nextjs Sentry.captureException for centralized exception tracing.

### Verify

```bash
# Discover and execute project linting, static analysis, and test suites to verify error logging and error boundary reporting
npm run lint || pnpm lint || bun lint
npm run test || pnpm test || bun test
```

**Accept when:**
- All client-side error boundary components invoke Sentry.captureException within error lifecycle hooks.
- Client-side RPC error dispatchers invoke Sentry.captureException with associated route extra metadata before propagation or warning.
- Static analysis checks and test suites confirm that unhandled client exceptions report to Sentry without missing contextual payloads.

<enforcement>
Claude Code MUST NOT skip or defer verification. All changes must be validated against static analysis and automated test suites confirming required Sentry exception capture calls are present.
</enforcement>