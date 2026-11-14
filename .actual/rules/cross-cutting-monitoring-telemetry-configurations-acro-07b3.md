---
version: 2
name: cross-cutting-monitoring-telemetry-configurations-acro-07b3
description: "@openstatus/api/src/trpc-errors Internal Module Adoption for Centralized Procedure Call Error Handling: Monitoring Telemetry Configurations Across Server Edge"
metadata:
  actual:
    adr_id: 07b38a44-24bc-4577-bc11-6a7788c4c682
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

# @openstatus/api/src/trpc-errors Internal Module Adoption for Centralized Procedure Call Error Handling: Monitoring Telemetry Configurations Across Server Edge

Scope: applies to `apps/status-page/sentry.edge.config.ts`, `apps/status-page/sentry.server.config.ts`, `apps/status-page/src/app/api/trpc/lambda/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TRPC-001** SHOULD: Monitoring and telemetry configurations across server and edge runtimes SHOULD use the exported error types from @openstatus/api/src/trpc-errors to filter and categorize operational exceptions.

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
Claude Code MUST NOT skip or defer verification. Automated dependency analysis, static type checking, and peer code review enforce compliance.
</enforcement>