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

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-RPC-001** MUST: Remote procedure call client error handlers MUST provide contextual diagnostic data including the target procedure path within the extra metadata parameter when invoking Sentry.captureException.

### Verify

```bash
# Discover dependency manifest, lock artifact, and verify the resolved version of @sentry/nextjs
if [ -f "package.json" ]; then
  npm list @sentry/nextjs || pnpm ls @sentry/nextjs || yarn list --pattern @sentry/nextjs
fi

# Discover and execute project static analysis and lint command
if npm run | grep -q "lint"; then
  npm run lint
elif pnpm run | grep -q "lint"; then
  pnpm lint
elif yarn run | grep -q "lint"; then
  yarn lint
fi

# Discover and run test suite
if npm run | grep -q "test"; then
  npm test
elif pnpm run | grep -q "test"; then
  pnpm test
elif yarn run | grep -q "test"; then
  yarn test
fi
```

**Accept when:**
- All global runtime error boundaries and remote procedure call error handlers invoke Sentry.captureException when errors occur.
- Remote procedure call error capture includes structured contextual metadata containing the procedure path.
- Repository verification and static analysis suites pass without unhandled exception reporting violations.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>