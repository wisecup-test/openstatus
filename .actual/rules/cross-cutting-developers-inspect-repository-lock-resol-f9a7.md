---
version: 2
name: cross-cutting-developers-inspect-repository-lock-resol-f9a7
description: "@sentry/nextjs Client Exception Capture and Error Tracing: Developers Inspect Repository Lock Resolution Artifact"
metadata:
  actual:
    adr_id: f9a77663-db37-4225-9767-1d1cbe7c568a
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

# @sentry/nextjs Client Exception Capture and Error Tracing: Developers Inspect Repository Lock Resolution Artifact

Scope: applies to `apps/dashboard/src/app/**`, `apps/dashboard/src/lib/trpc/**`.

These rules are ALWAYS ACTIVE for all client-side user interface error boundary components and shared client-side RPC and API communication error handlers.

### Rules

- **R-SENTRY-001** MUST: Developers MUST inspect the repository lock or resolution artifact to determine the exact resolved version of @sentry/nextjs before implementing or modifying error capture calls.
- **R-SENTRY-002** MUST: Client-side error boundary components MUST invoke Sentry.captureException within error lifecycle hooks.
- **R-SENTRY-003** MUST: Client-side RPC error dispatchers MUST invoke Sentry.captureException with associated route extra metadata before propagation or warning.
- **R-SENTRY-004** MUST: Configure client-side telemetry scrubbing hooks to remove credentials, tokens, or personal identifiers before event transmission.

### Verify

```bash
# Discover and execute the project linting and static analysis scripts to verify adherence to error logging standards
# Discover and run the project automated test suite to ensure error boundary reporting behavior is validated
npm test || yarn test || pnpm test
```

**Accept when:**
- All client-side error boundary components invoke Sentry.captureException within error lifecycle hooks.
- Client-side RPC error dispatchers invoke Sentry.captureException with associated route extra metadata before propagation or warning.
- Static analysis checks and test suites confirm that unhandled client exceptions report to Sentry without missing contextual payloads.

<enforcement>
Claude Code MUST NOT skip or defer verification. Pull requests missing Sentry error capture in error boundaries or client RPC handlers are blocked from merge.
</enforcement>