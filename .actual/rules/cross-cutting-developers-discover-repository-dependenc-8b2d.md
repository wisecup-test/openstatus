---
version: 2
name: cross-cutting-developers-discover-repository-dependenc-8b2d
description: "Adoption of @openstatus/analytics Module for Telemetry Dispatch: Developers Discover Repository Dependency Manifest Authoritative"
metadata:
  actual:
    adr_id: 8b2d1c6d-8ab5-46c3-ba00-e25e9ade295d
    status: accepted
    candidate_group_id: db333959-f17e-4609-93f0-608d127c3a05
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
---

# Adoption of @openstatus/analytics Module for Telemetry Dispatch: Developers Discover Repository Dependency Manifest Authoritative

Scope: applies to `apps/web/src/app/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ANA-001** MUST: Developers MUST discover the repository dependency manifest and authoritative lock artifact to verify the exact resolved version of @openstatus/analytics and its peer dependencies before implementing or modifying analytics integration.
- **R-ANA-002** MUST: Wrap telemetry client initialization and event tracking within post-response lifecycle handlers to avoid adding latency to critical-path HTTP responses.
- **R-ANA-003** MUST: Provide fallback error logging when analytics tracking fails so that telemetry failures do not interrupt primary execution flow.

### Verify

```bash
# Discover and run the project test runner script covering route telemetry
# Discover and run the workspace static analysis and type check script
# Discover and run the project code linter
```

**Accept when:**
- All unit and integration tests covering route telemetry pass with zero errors.
- Static type analysis verifies that all calls to setupAnalytics and analytics.track match the module signature without type suppression.
- Code analysis confirms that telemetry dispatches in route handlers are enclosed in exception-handling structures within post-response execution blocks.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration checks enforce type safety, unhandled promise rejection prevention, and exception isolation for analytics integration.
</enforcement>