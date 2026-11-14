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

These rules are ALWAYS ACTIVE for all application user interface error boundaries, fallback handlers, client, server, and edge execution initialization workflows, and remote procedure call client middleware and error handling handlers.

### Rules

- **R-TELEMETRY-001** MUST_NOT: Telemetry instrumentation MUST NOT capture or transmit sensitive credentials, authorization tokens, or internal environment configuration values.

### Verify

```bash
# Discover and execute compilation pipeline
# Discover and run test suite
# Discover static analysis configuration and run linter
```

**Accept when:**
- All application execution targets compile cleanly with telemetry initialization logic included.
- Test suites pass with error boundary and network client error capture assertions satisfied.
- Static analysis checks pass without errors or unauthorized telemetry patterns.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration automated test suites, automated linting, and peer code reviews enforce these requirements.
</enforcement>