---
version: 2
name: cross-cutting-shared-error-definitions-openstatus-src-8fd3
description: "@openstatus/api/src/trpc-errors Internal Module Adoption for Centralized Procedure Call Error Handling: Shared Error Definitions Openstatus Src Trpc"
metadata:
  actual:
    adr_id: 8fd3a811-0470-4047-bfe1-41b79982d41f
    status: accepted
    candidate_group_id: 0c61fac3-cc9d-4ec8-a1e7-b15a3bb62c76
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - sentry.edge.config.ts
      - sentry.server.config.ts
      - src/app/api/trpc/lambda/**
---

# @openstatus/api/src/trpc-errors Internal Module Adoption for Centralized Procedure Call Error Handling: Shared Error Definitions Openstatus Src Trpc

Scope: applies to `apps/status-page/sentry.edge.config.ts`, `apps/status-page/sentry.server.config.ts`, `apps/status-page/src/app/api/trpc/lambda/**`.

These rules are ALWAYS ACTIVE for all API routes, endpoint adapters, and runtime observability/monitoring configuration modules across server and edge environments.

### Rules

- **R-ERR-001** MUST: Shared error definitions in @openstatus/api/src/trpc-errors MUST remain isomorphic and executable across both server and edge runtime targets without environment-specific side effects.

### Verify

```bash
discover_and_execute_typecheck_validation
discover_and_execute_module_dependency_lint
discover_and_execute_runtime_test_suite
```

**Accept when:**
- All modules importing remote procedure call error classifications resolve definitions exclusively from @openstatus/api/src/trpc-errors.
- Static analysis and type validation confirm zero direct instantiations of untyped protocol errors in application handlers and runtime monitoring configurations.
- Build and runtime initialization across server and edge environments pass without module resolution errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>