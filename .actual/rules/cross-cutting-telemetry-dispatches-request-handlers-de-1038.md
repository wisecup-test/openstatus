---
version: 2
name: cross-cutting-telemetry-dispatches-request-handlers-de-1038
description: "Adoption of @openstatus/analytics Module for Telemetry Dispatch: Telemetry Dispatches Request Handlers Deferred Background"
metadata:
  actual:
    adr_id: 1038ebb6-9392-49db-9751-3a0f14c53fe5
    status: accepted
    candidate_group_id: db333959-f17e-4609-93f0-608d127c3a05
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
---

# Adoption of @openstatus/analytics Module for Telemetry Dispatch: Telemetry Dispatches Request Handlers Deferred Background

Scope: applies to `apps/web/src/app/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TELEMETRY-001** SHOULD: Telemetry dispatches in request handlers SHOULD be deferred to background post-response execution using after to isolate telemetry latency from client response times.
- **R-TELEMETRY-002** MANDATORY: Wrap telemetry client initialization and event tracking within post-response lifecycle handlers and enforce explicit error trapping/logging around analytics client initialization and dispatch calls.
- **R-TELEMETRY-003** MANDATORY: Enforce lock-version grounding before modifying or invoking module interfaces by finding the dependency manifest, identifying the build tool, inspecting the lock file for the exact resolved version, checking public API documentation, and confirming API signatures.

### Verify

```bash
# Discover and run test runner script covering route telemetry
# Discover and run workspace static analysis and type check script
# Discover and run project code linter
```

**Accept when:**
- All unit and integration tests covering route telemetry pass with zero errors.
- Static type analysis verifies that all calls to setupAnalytics and analytics.track match the module signature without type suppression.
- Code analysis confirms that telemetry dispatches in route handlers are enclosed in exception-handling structures within post-response execution blocks.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>