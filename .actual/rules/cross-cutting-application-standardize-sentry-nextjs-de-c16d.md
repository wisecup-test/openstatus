---
version: 2
name: cross-cutting-application-standardize-sentry-nextjs-de-c16d
description: "@sentry/nextjs Adoption for Cross-Runtime Error Tracking and Observability: Application Standardize Sentry Nextjs Dedicated Telemetry"
metadata:
  actual:
    adr_id: c16d2034-58d5-4038-b4c3-099b094c8f2a
    status: accepted
    candidate_group_id: 002e37f3-4d0a-434f-ae9a-10f3bc6c6348
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - instrumentation-client.ts
      - next.config.ts
      - sentry.edge.config.ts
      - sentry.server.config.ts
      - src/**
---

# @sentry/nextjs Adoption for Cross-Runtime Error Tracking and Observability: Application Standardize Sentry Nextjs Dedicated Telemetry

Scope: applies to `apps/dashboard/instrumentation-client.ts`, `apps/dashboard/next.config.ts`, `apps/dashboard/sentry.edge.config.ts`, `apps/dashboard/sentry.server.config.ts`, `apps/dashboard/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SENTRY-001** MUST: The application MUST standardize on @sentry/nextjs as the dedicated telemetry library for recording runtime anomalies, exceptions, and error tracing across all application execution environments.

### Verify

```bash
# Discover and run the build script to verify telemetry instrumentation compiles without type errors across all runtime targets
npm run build
# Discover and run the project test suite to verify error boundary components and remote procedure call handlers correctly execute exception capture calls
npm test
# Discover and run the repository linter to verify compliance with telemetry conventions and absence of direct console logging
npm run lint
```

**Accept when:**
- All application execution targets compile cleanly with telemetry initialization logic included.
- Test suites pass with error boundary and network client error capture assertions satisfied.
- Static analysis checks pass without errors or unauthorized telemetry patterns.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>