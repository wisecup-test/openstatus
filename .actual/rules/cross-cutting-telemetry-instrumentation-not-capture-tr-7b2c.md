---
version: 2
name: cross-cutting-telemetry-instrumentation-not-capture-tr-7b2c
description: "@sentry/nextjs Adoption for Cross-Runtime Error Tracking and Observability: Telemetry Instrumentation Not Capture Transmit Sensitive"
metadata:
  actual:
    adr_id: 7b2cfa06-e94f-4c66-8748-b6d63502e611
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

# @sentry/nextjs Adoption for Cross-Runtime Error Tracking and Observability: Telemetry Instrumentation Not Capture Transmit Sensitive

Scope: applies to `apps/dashboard/instrumentation-client.ts`, `apps/dashboard/next.config.ts`, `apps/dashboard/sentry.edge.config.ts`, `apps/dashboard/sentry.server.config.ts`, `apps/dashboard/src/**`.

These rules are ALWAYS ACTIVE for all application user interface error boundaries, fallback handlers, client/server/edge initialization workflows, and remote procedure call client middleware.

### Rules

- **R-SENTRY-001** MUST_NOT: Telemetry instrumentation MUST NOT capture or transmit sensitive credentials, authorization tokens, or internal environment configuration values.

### Verify

```bash
# Discover the repository build script from the dependency manifest and execute the compilation pipeline to verify that telemetry instrumentation compiles without type errors across all runtime targets.
# Discover and run the project test suite to verify that error boundary components and remote procedure call handlers correctly execute exception capture calls.
# Discover the project static analysis configuration and run the repository linter to verify compliance with telemetry conventions and absence of direct console logging.
```

**Accept when:**
- All application execution targets compile cleanly with telemetry initialization logic included.
- Test suites pass with error boundary and network client error capture assertions satisfied.
- Static analysis checks pass without errors or unauthorized telemetry patterns.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>