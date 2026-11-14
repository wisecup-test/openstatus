---
version: 2
name: cross-cutting-applications-not-implement-competing-thi-a0db
description: "Adoption of @sentry/nextjs for Runtime Observability and Error Monitoring: Applications Not Implement Competing Third Party"
metadata:
  actual:
    adr_id: a0dbc1f3-5e0d-4cd7-b221-5b5fcc0eb52e
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

# Adoption of @sentry/nextjs for Runtime Observability and Error Monitoring: Applications Not Implement Competing Third Party

Scope: applies to `apps/status-page/instrumentation-client.ts`, `apps/status-page/next.config.ts`, `apps/status-page/sentry.edge.config.ts`, `apps/status-page/sentry.server.config.ts`, `apps/status-page/src/**`.

These rules are ALWAYS ACTIVE for all applications deployed across heterogeneous execution runtimes requiring a unified exception monitoring and telemetry capture mechanism.

### Rules

- **R-SENTRY-001** MUST_NOT: Applications MUST_NOT implement competing third-party error tracking SDKs or ad-hoc custom telemetry capture clients that duplicate error dispatching.

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