---
version: 2
name: cross-cutting-telemetry-tracking-instantiate-analytics-acb2
description: "Adoption of @openstatus/analytics Module for Telemetry Dispatch: Telemetry Tracking Instantiate Analytics Client Via"
metadata:
  actual:
    adr_id: acb27686-13d8-4f8f-b760-91c808e4a631
    status: accepted
    candidate_group_id: db333959-f17e-4609-93f0-608d127c3a05
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
---

# Adoption of @openstatus/analytics Module for Telemetry Dispatch: Telemetry Tracking Instantiate Analytics Client Via

Scope: applies to `apps/web/src/app/**`.

These rules are ALWAYS ACTIVE for API route handlers requiring asynchronous telemetry and event instrumentation.

### Rules

- **R-TELEMETRY-001** MUST: Telemetry tracking MUST instantiate the analytics client via setupAnalytics and dispatch structured events via analytics.track.

### Verify

```bash
# Discover and run the project test runner script covering route telemetry
npm test -- --grep telemetry

# Discover and run workspace static analysis and type check
npm run typecheck

# Discover and run the project code linter
npm run lint
```

**Accept when:**
- All unit and integration tests covering route telemetry pass with zero errors.
- Static type analysis verifies that all calls to setupAnalytics and analytics.track match the module signature without type suppression.
- Code analysis confirms that telemetry dispatches in route handlers are enclosed in exception-handling structures within post-response execution blocks.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration checks execute project test suites and type checkers to enforce compliance.
</enforcement>