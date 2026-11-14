---
version: 2
name: cross-cutting-remote-procedure-call-client-error-fbee
description: "Exception Capture and Error Tracing via @sentry/nextjs: Remote Procedure Call Client Error Handlers"
metadata:
  actual:
    adr_id: fbeec4d7-3ce6-478e-a6c1-8600676c4034
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

# Exception Capture and Error Tracing via @sentry/nextjs: Remote Procedure Call Client Error Handlers

Scope: applies to `apps/web/src/app/**`, `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for all client-side global error boundary components and remote procedure call client communication error handling callbacks and link configurations matching the configured scope.

### Rules

- **R-RPC-001** MUST: Remote procedure call client error handlers MUST provide contextual diagnostic data including the target procedure path within the extra metadata parameter when invoking Sentry.captureException.

### Verify

```bash
# Discover the repository dependency manifest and lock artifact, and verify the resolved version of @sentry/nextjs.
if [ -f "package.json" ]; then
  npm ls @sentry/nextjs || pnpm ls @sentry/nextjs || yarn ls @sentry/nextjs
fi

# Discover and execute project static analysis and lint commands
if grep -q 'lint' package.json; then
  npm run lint || pnpm lint || yarn lint
fi

# Discover and run the project test suite
if grep -q 'test' package.json; then
  npm test || pnpm test || yarn test
fi
```

**Accept when:**
- All global runtime error boundaries and remote procedure call error handlers invoke Sentry.captureException when errors occur.
- Remote procedure call error capture includes structured contextual metadata containing the procedure path.
- Repository verification and static analysis suites pass without unhandled exception reporting violations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Compliance with these rules is mandatory and enforced via automated static analysis, test suites, and pull request reviews.
</enforcement>