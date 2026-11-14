---
version: 2
name: cross-cutting-openstatus-src-trpc-errors-module-ad14
description: "Adoption of @openstatus/api/src/trpc-errors for Centralized Error Handling: Openstatus Src Trpc Errors Module Maintain"
metadata:
  actual:
    adr_id: ad144a68-5e5b-4bb5-a1d1-5b867e62497e
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

# Adoption of @openstatus/api/src/trpc-errors for Centralized Error Handling: Openstatus Src Trpc Errors Module Maintain

Scope: applies to `apps/web/sentry.edge.config.ts`, `apps/web/sentry.server.config.ts`, `apps/web/src/app/api/trpc/lambda/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OPENSTATUS-TRPC-ERR-001** MUST: The @openstatus/api/src/trpc-errors module MUST maintain compatibility across both server and edge runtime execution environments.

### Verify

```bash
# Discover the project verification scripts from the project manifest and execute the static analysis suite to verify error module import compliance.
# Execute the project test suite using the project build tool to verify error classification behavior across all runtime targets.
```

**Accept when:**
- All application endpoints and runtime monitoring configurations import error classification structures exclusively from the shared error module.
- Static analysis and repository verification checks pass with zero unresolved error type references across server and edge environments.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis rules and peer code reviews verify import paths for error classification utilities.
</enforcement>