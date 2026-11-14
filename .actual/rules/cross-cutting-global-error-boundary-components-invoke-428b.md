---
version: 2
name: cross-cutting-global-error-boundary-components-invoke-428b
description: "@sentry/nextjs Exception Capture and Boundary Tracing: Global Error Boundary Components Invoke Sentry"
metadata:
  actual:
    adr_id: 428bb3d9-53a9-4739-abba-a5ad71e109e5
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

# @sentry/nextjs Exception Capture and Boundary Tracing: Global Error Boundary Components Invoke Sentry

Scope: applies to `apps/status-page/src/app/**`, `apps/status-page/src/lib/trpc/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SENTRY-001** SHOULD: Global UI error boundary components SHOULD invoke Sentry.captureException within an execution lifecycle effect upon receiving an unhandled component rendering error.

### Verify

```bash
# Discover the repository script runner and execute the static analysis check for tracing boundary compliance.
# Run the test suite target responsible for validating client error boundary exception dispatching.
npm test
```

**Accept when:**
- Static analysis confirms that unhandled boundary error handlers invoke Sentry.captureException.
- Automated verification passes with contextual path metadata validated in remote call error interceptors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification by automated static analysis checks and peer code reviews is mandatory.
</enforcement>