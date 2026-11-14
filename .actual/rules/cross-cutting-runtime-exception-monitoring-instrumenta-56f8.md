---
version: 2
name: cross-cutting-runtime-exception-monitoring-instrumenta-56f8
description: "Adoption of @openstatus/api/src/trpc-errors for Centralized Error Handling: Runtime Exception Monitoring Instrumentation Extract Error"
metadata:
  actual:
    adr_id: 56f82c1c-7990-450d-a878-ec9704709e70
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

# Adoption of @openstatus/api/src/trpc-errors for Centralized Error Handling: Runtime Exception Monitoring Instrumentation Extract Error

Scope: applies to `apps/web/sentry.edge.config.ts`, `apps/web/sentry.server.config.ts`, `apps/web/src/app/api/trpc/lambda/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ERR-001** SHOULD: Runtime exception monitoring instrumentation SHOULD extract error codes and operational metadata using helper utilities exported by @openstatus/api/src/trpc-errors.

### Verify

```bash
# Discover project verification scripts from manifest and execute static analysis
# Execute project test suite using the project build tool
```

**Accept when:**
- All application endpoints and runtime monitoring configurations import error classification structures exclusively from the shared error module.
- Static analysis and repository verification checks pass with zero unresolved error type references across server and edge environments.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests must use centralized error classification and avoid decentralized or duplicated error handling.
</enforcement>