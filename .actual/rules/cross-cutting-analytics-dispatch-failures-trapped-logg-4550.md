---
version: 2
name: cross-cutting-analytics-dispatch-failures-trapped-logg-4550
description: "Adoption of @openstatus/analytics Module for Telemetry Dispatch: Analytics Dispatch Failures Trapped Logged Without"
metadata:
  actual:
    adr_id: 45507819-387b-4d6a-9ca9-56aa6569fad8
    status: accepted
    candidate_group_id: db333959-f17e-4609-93f0-608d127c3a05
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
---

# Adoption of @openstatus/analytics Module for Telemetry Dispatch: Analytics Dispatch Failures Trapped Logged Without

Scope: applies to `apps/web/src/app/**`.

These rules are ALWAYS ACTIVE for API route handlers requiring asynchronous telemetry and event instrumentation.

### Rules

- **R-ANA-001** MUST: Analytics dispatch failures MUST be trapped and logged without failing the enclosing request or throwing unhandled rejections.
- **R-ANA-002** MUST: Execute telemetry within post-response execution blocks to isolate telemetry failures from request resolution and avoid adding network latency to client responses.
- **R-ANA-003** MUST: Perform lock-version grounding before writing code that uses a versioned library by inspecting the dependency manifest, identifying the build tool, verifying the exact resolved version via the lock artifact, looking up official documentation, and confirming every API call exists.

### Verify

```bash
# Discover and run the unit test suite covering route telemetry
# Discover and run workspace static analysis and type check
# Discover and run the project code linter
```

**Accept when:**
- All unit and integration tests covering route telemetry pass with zero errors.
- Static type analysis verifies that all calls to setupAnalytics and analytics.track match the module signature without type suppression.
- Code analysis confirms that telemetry dispatches in route handlers are enclosed in exception-handling structures within post-response execution blocks.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>