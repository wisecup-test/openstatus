---
version: 2
name: cross-cutting-client-error-logging-mechanisms-compleme-73fa
description: "Exception Capture and Error Tracing via @sentry/nextjs: Client Error Logging Mechanisms Complement Remote"
metadata:
  actual:
    adr_id: 73faccc7-391d-41e1-a92e-9c73575c6206
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

# Exception Capture and Error Tracing via @sentry/nextjs: Client Error Logging Mechanisms Complement Remote

Scope: applies to `apps/web/src/app/**`, `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SENTRY-001** SHOULD: Client error logging mechanisms SHOULD complement remote exception capture with local warning messages without exposing sensitive payload details.
- **R-SENTRY-002** MANDATORY: Client-side global error boundary components handling uncaught runtime exceptions and all remote procedure call client communication error handling callbacks and link configurations MUST invoke the exception capture API directly inside component lifecycle hooks or effect handlers.
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
Verification is mandatory. Claude Code MUST NOT skip or defer verification.
</enforcement>