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

- **R-TEL-001** MUST: Route handlers requiring telemetry dispatch MUST import and use the @openstatus/analytics module for event tracking rather than communicating directly with external telemetry sinks.
- **R-TEL-002** MUST: Wrap telemetry client initialization and event tracking within post-response lifecycle handlers to avoid adding latency to critical-path HTTP responses.
- **R-TEL-003** MUST: Provide fallback error logging when analytics tracking fails so that telemetry failures do not interrupt primary execution flow.
- **R-TEL-004** MUST: Enforce explicit error trapping and logging around analytics client initialization and dispatch calls to prevent unhandled promise rejections.
- **R-TEL-005** MUST: Enforce lock-file version grounding before modifying or invoking module interfaces.

### Verify

```bash
# Discover and execute the project test runner script covering route telemetry
# Discover and run the workspace static analysis and type check script
# Discover and run the project code linter
```

**Accept when:**
- All unit and integration tests covering route telemetry pass with zero errors.
- Static type analysis verifies that all calls to setupAnalytics and analytics.track match the module signature without type suppression.
- Code analysis confirms that telemetry dispatches in route handlers are enclosed in exception-handling structures within post-response execution blocks.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration checks, static analysis, and code reviews will enforce telemetry isolation and error handling compliance.
</enforcement>