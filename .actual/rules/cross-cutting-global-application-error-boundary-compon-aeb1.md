---
version: 2
name: cross-cutting-global-application-error-boundary-compon-aeb1
description: "Exception Capture and Error Tracing via @sentry/nextjs: Global Application Error Boundary Components Invoke"
metadata:
  actual:
    adr_id: aeb1bbcf-8cb1-4517-a6ee-bd59de3048cb
    status: accepted
    candidate_group_id: 0aa494d9-538b-4b0c-a5ed-a28a60aec767
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
      - src/trpc/**
---

# Exception Capture and Error Tracing via @sentry/nextjs: Global Application Error Boundary Components Invoke

Scope: applies to `apps/web/src/app/**`, `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for all client-side global error boundary components handling uncaught runtime exceptions and remote procedure call client communication error handling callbacks and link configurations.

### Rules

- **R-EX-001** MUST: Global application error boundary components MUST invoke Sentry.captureException with the caught error object within the component mounting or effect lifecycle.

### Verify

```bash
# Discover dependency manifest/lock and check resolved @sentry/nextjs version
# (Repository-dependent discovery)

# Run static analysis and lint command to verify compliance
# (Repository-dependent command, e.g., npm run lint / npx tsc)

# Run test suite to verify error boundary and procedure error handler test cases
# (Repository-dependent command, e.g., npm test)
```

**Accept when:**
- All global runtime error boundaries and remote procedure call error handlers invoke Sentry.captureException when errors occur.
- Remote procedure call error capture includes structured contextual metadata containing the procedure path.
- Repository verification and static analysis suites pass without unhandled exception reporting violations.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>