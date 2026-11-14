---
version: 2
name: cross-cutting-health-check-monitoring-endpoints-expose-dee0
description: "Gin Framework Service Boundary Definition and Context Propagation: Health Check Monitoring Endpoints Exposed Via"
metadata:
  actual:
    adr_id: dee0c430-ffe0-4d19-b368-3a5f70bed63f
    status: accepted
    candidate_group_id: 35868210-db45-458b-9030-71b77645c31c
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - checker/**
      - cmd/server/**
      - handlers/**
      - pkg/otel/**
      - pkg/tinybird/**
---

# Gin Framework Service Boundary Definition and Context Propagation: Health Check Monitoring Endpoints Exposed Via

Scope: applies to `apps/checker/checker/**`, `apps/checker/cmd/server/**`, `apps/checker/handlers/**`, `apps/checker/pkg/otel/**`, `apps/checker/pkg/tinybird/**`.

These rules are ALWAYS ACTIVE for all HTTP server entrypoints, route registration components, boundary handler functions, and middleware components managing request-scoped metadata and correlation state.

### Rules

- **R-GIN-001** MUST: Health check and monitoring endpoints MUST be exposed via router bindings returning standard JSON responses with appropriate HTTP status codes.

### Verify

```bash
# Inspect the project manifest to identify the configured test runner and execute test suites covering API boundary handlers
# Execute repository static analysis and linting tools to verify route definitions and context usage compliance
```

**Accept when:**
- All service boundary routes compile and pass automated test suites covering HTTP and protocol checking endpoints.
- Context metadata propagation operations complete without runtime type assertion failures or missing key errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks, test suites, static analysis, and mandatory peer code reviews enforce compliance.
</enforcement>