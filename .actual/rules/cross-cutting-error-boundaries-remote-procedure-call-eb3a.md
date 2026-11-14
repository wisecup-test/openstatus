---
version: 2
name: cross-cutting-error-boundaries-remote-procedure-call-eb3a
description: "Exception Capture and Error Tracing via @sentry/nextjs: Error Boundaries Remote Procedure Call Callbacks"
metadata:
  actual:
    adr_id: eb3aa60e-71ca-4869-9af0-89f1fa251ecf
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

# Exception Capture and Error Tracing via @sentry/nextjs: Error Boundaries Remote Procedure Call Callbacks

Scope: applies to `apps/web/src/app/**`, `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for all client-side global error boundary components handling uncaught runtime exceptions and all remote procedure call client communication error handling callbacks and link configurations.

### Rules

- **R-SENTRY-001** MUST_NOT: Error boundaries and remote procedure call error callbacks MUST NOT suppress or swallow uncaught exceptions without recording them through Sentry.captureException.
- **R-SENTRY-002** MANDATORY: Invoke Sentry.captureException directly inside component lifecycle hooks or effect handlers dedicated to uncaught error capture.
- **R-SENTRY-003** MANDATORY: When handling remote procedure call client failures, pass structured metadata containing the target procedure path within the extra context argument.
- **R-SENTRY-004** MANDATORY: Ensure sensitive authentication credentials or personally identifiable information are omitted or scrubbed prior to dispatching exception metadata.

### Verify

```bash
# Discover the repository dependency manifest and lock artifact, and verify the resolved version of @sentry/nextjs.
# Discover and execute the project static analysis and lint command to verify compliance with Sentry.captureException rules across error boundaries and RPC clients.
# Discover and run the project test suite to verify that error boundary and procedure error handler test cases successfully dispatch error tracing calls.
```

**Accept when:**
- All global runtime error boundaries and remote procedure call error handlers invoke Sentry.captureException when errors occur.
- Remote procedure call error capture includes structured contextual metadata containing the procedure path.
- Repository verification and static analysis suites pass without unhandled exception reporting violations.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests and changes introducing error boundary catches or RPC error listeners that omit Sentry.captureException must be rejected by static analysis and continuous integration pipelines.
</enforcement>