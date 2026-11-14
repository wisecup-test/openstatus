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

These rules are ALWAYS ACTIVE for API route handlers requiring asynchronous telemetry and event instrumentation.

### Rules

- **R-TEL-001** SHOULD: Telemetry dispatches in request handlers SHOULD be deferred to background post-response execution using after to isolate telemetry latency from client response times.
- **R-TEL-002** MANDATORY: Wrap telemetry client initialization and event tracking within post-response lifecycle handlers to avoid adding latency to critical-path HTTP responses, and provide fallback error logging when analytics tracking fails.
- **R-TEL-003** MANDATORY (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, inspect the dependency manifest, identify the build tool, inspect the lock/resolution artifact to determine the exact resolved version, look up official documentation for that exact version, and confirm every API/class/function exists before calling it.

### Verify

```bash
# Discover and run the repository test runner script for route telemetry
# Discover and run the workspace static analysis and type check script
# Discover and run the project code linter
```

**Accept when:**
- All unit and integration tests covering route telemetry pass with zero errors.
- Static type analysis verifies that all calls to setupAnalytics and analytics.track match the module signature without type suppression.
- Code analysis confirms that telemetry dispatches in route handlers are enclosed in exception-handling structures within post-response execution blocks.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration checks and code reviews verify rule compliance.
</enforcement>