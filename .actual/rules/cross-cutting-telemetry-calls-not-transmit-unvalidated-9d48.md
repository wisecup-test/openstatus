---
version: 2
name: cross-cutting-telemetry-calls-not-transmit-unvalidated-9d48
description: "Adoption of @openstatus/analytics Module for Telemetry Dispatch: Telemetry Calls Not Transmit Unvalidated Raw"
metadata:
  actual:
    adr_id: 9d48646e-7500-45ad-819f-688f5f48d9bb
    status: accepted
    candidate_group_id: db333959-f17e-4609-93f0-608d127c3a05
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
---

# Adoption of @openstatus/analytics Module for Telemetry Dispatch: Telemetry Calls Not Transmit Unvalidated Raw

Scope: applies to `apps/web/src/app/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TEL-001** MUST_NOT: Telemetry calls MUST NOT transmit unvalidated or raw sensitive client payload structures.

### Verify

```bash
# Discover and run the package test runner script covering route telemetry
# Discover and run the workspace static analysis and type check script
# Discover and run the project code linter
```

**Accept when:**
- All unit and integration tests covering route telemetry pass with zero errors.
- Static type analysis verifies that all calls to setupAnalytics and analytics.track match the module signature without type suppression.
- Code analysis confirms that telemetry dispatches in route handlers are enclosed in exception-handling structures within post-response execution blocks.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration checks and code reviews strictly enforce these rules, causing failures for type mismatches, unhandled promise rejections, or unencapsulated telemetry calls.
</enforcement>