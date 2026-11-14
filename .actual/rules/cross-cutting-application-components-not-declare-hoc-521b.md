---
version: 2
name: cross-cutting-application-components-not-declare-hoc-521b
description: "@openstatus/api/src/trpc-errors Internal Module Adoption for Centralized Procedure Call Error Handling: Application Components Not Declare Hoc Duplicated"
metadata:
  actual:
    adr_id: 521b4157-6ac5-40ba-a8b8-6423fcffa636
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

# @openstatus/api/src/trpc-errors Internal Module Adoption for Centralized Procedure Call Error Handling: Application Components Not Declare Hoc Duplicated

Scope: applies to `apps/status-page/sentry.edge.config.ts`, `apps/status-page/sentry.server.config.ts`, `apps/status-page/src/app/api/trpc/lambda/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ERR-001** MUST_NOT: Application components MUST NOT declare ad-hoc or duplicated error type hierarchies that bypass the shared definitions exported by @openstatus/api/src/trpc-errors.

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
Claude Code MUST NOT skip or defer verification. All pull requests introducing decentralized error definitions must fail CI.
</enforcement>