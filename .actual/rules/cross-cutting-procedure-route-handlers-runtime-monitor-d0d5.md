---
version: 2
name: cross-cutting-procedure-route-handlers-runtime-monitor-d0d5
description: "Adoption of @openstatus/api/src/trpc-errors for Centralized Error Handling: Procedure Route Handlers Runtime Monitoring Configurations"
metadata:
  actual:
    adr_id: d0d59f6a-eb46-4347-9c66-0c2e0a55bee0
    status: accepted
    candidate_group_id: 20a80ba6-7010-4233-887b-c7e52e94f2c0
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - sentry.edge.config.ts
      - sentry.server.config.ts
      - src/app/api/trpc/lambda/**
---

# Adoption of @openstatus/api/src/trpc-errors for Centralized Error Handling: Procedure Route Handlers Runtime Monitoring Configurations

Scope: applies to `apps/web/sentry.edge.config.ts`, `apps/web/sentry.server.config.ts`, `apps/web/src/app/api/trpc/lambda/**`.

These rules are ALWAYS ACTIVE for all procedure route handlers and runtime monitoring configurations across server and edge runtimes.

### Rules

- **R-OPENSTATUS-001** MUST: All procedure route handlers and runtime monitoring configurations MUST import protocol error definitions and classification utilities directly from @openstatus/api/src/trpc-errors.

### Verify

```bash
# Discover and execute project static analysis and test verification commands from the project manifest
# Example pattern:
# npm run lint
# npm test
```

**Accept when:**
- All application endpoints and runtime monitoring configurations import error classification structures exclusively from the shared error module.
- Static analysis and repository verification checks pass with zero unresolved error type references across server and edge environments.

<enforcement>
Claude Code MUST NOT skip or defer verification. Violations must be refactored to consume the centralized error handling module prior to merge.
</enforcement>