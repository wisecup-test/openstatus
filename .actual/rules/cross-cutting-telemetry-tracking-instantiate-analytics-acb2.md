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

- **R-OT-001** MUST: Telemetry tracking MUST instantiate the analytics client via setupAnalytics and dispatch structured events via analytics.track.

### Verify

```bash
# Discover the repository test runner script from the package configuration and execute the unit test suite covering route telemetry.
# Discover the workspace static analysis and type check script to verify that module imports and tracking event structures conform to the internal analytics schema.
# Discover and run the project code linter to verify compliance with exception handling and asynchronous execution rules.
```

**Accept when:**
- All unit and integration tests covering route telemetry pass with zero errors.
- Static type analysis verifies that all calls to setupAnalytics and analytics.track match the module signature without type suppression.
- Code analysis confirms that telemetry dispatches in route handlers are enclosed in exception-handling structures within post-response execution blocks.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>