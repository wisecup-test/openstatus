---
version: 2
name: cross-cutting-invocations-sentry-captureexception-atta-bc37
description: "Adoption of @sentry/nextjs for Runtime Observability and Error Monitoring: Invocations Sentry Captureexception Attach Contextual Metadata"
metadata:
  actual:
    adr_id: bc37b944-b2b6-45ef-8eee-a6dfb9964ed1
    status: accepted
    candidate_group_id: f9ac9ba3-1eee-4b96-86b5-30eaf058325a
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - instrumentation-client.ts
      - next.config.ts
      - sentry.edge.config.ts
      - sentry.server.config.ts
      - src/**
---

# Adoption of @sentry/nextjs for Runtime Observability and Error Monitoring: Invocations Sentry Captureexception Attach Contextual Metadata

Scope: applies to `apps/status-page/instrumentation-client.ts`, `apps/status-page/next.config.ts`, `apps/status-page/sentry.edge.config.ts`, `apps/status-page/sentry.server.config.ts`, `apps/status-page/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SENTRY-001** SHOULD: Invocations of Sentry.captureException SHOULD attach contextual metadata and path identifiers to the error payload to facilitate diagnostic tracing.

### Verify

```bash
# Discover and run the project linting and static analysis scripts from the repository manifest
# Discover and run the project build script to verify multi-runtime compilation and instrumentation
# Discover and run the project automated test suite to ensure error boundaries and handlers pass
```

**Accept when:**
- Static analysis and compilation complete across all target runtimes with zero errors.
- All error boundary and remote procedure call client tests pass successfully.
- Telemetry instrumentation initializes without throwing unhandled exceptions across server, edge, and client environments.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>