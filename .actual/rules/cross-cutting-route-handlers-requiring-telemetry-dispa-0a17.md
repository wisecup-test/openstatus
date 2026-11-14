---
version: 2
name: cross-cutting-route-handlers-requiring-telemetry-dispa-0a17
description: "Adoption of @openstatus/analytics Module for Telemetry Dispatch: Route Handlers Requiring Telemetry Dispatch Import"
metadata:
  actual:
    adr_id: 0a176d18-6e9a-41a2-a249-1bb837b389d2
    status: accepted
    candidate_group_id: db333959-f17e-4609-93f0-608d127c3a05
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
---

# Adoption of @openstatus/analytics Module for Telemetry Dispatch: Route Handlers Requiring Telemetry Dispatch Import

Scope: applies to `apps/web/src/app/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TELEMETRY-001** MUST: Route handlers requiring telemetry dispatch MUST import and use the @openstatus/analytics module for event tracking rather than communicating directly with external telemetry sinks.

### Verify

```bash
# Discover and run the workspace test runner, type check, and linter scripts from package configuration
# Example derivation:
# npm test
# npm run typecheck
# npm run lint
```

**Accept when:**
- All unit and integration tests covering route telemetry pass with zero errors.
- Static type analysis verifies that all calls to setupAnalytics and analytics.track match the module signature without type suppression.
- Code analysis confirms that telemetry dispatches in route handlers are enclosed in exception-handling structures within post-response execution blocks.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>