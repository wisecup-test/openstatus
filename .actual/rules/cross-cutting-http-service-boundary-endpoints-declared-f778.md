---
version: 2
name: cross-cutting-http-service-boundary-endpoints-declared-f778
description: "Gin Framework Service Boundary Definition and Context Propagation: Http Service Boundary Endpoints Declared Gin"
metadata:
  actual:
    adr_id: f778bc89-2cc5-4152-9363-8694d72d92ef
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

# Gin Framework Service Boundary Definition and Context Propagation: Http Service Boundary Endpoints Declared Gin

Scope: applies to `apps/checker/checker/**`, `apps/checker/cmd/server/**`, `apps/checker/handlers/**`, `apps/checker/pkg/otel/**`, `apps/checker/pkg/tinybird/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-GIN-001** MUST: All HTTP service boundary endpoints MUST be declared using Gin router method bindings with handlers accepting the framework context pointer as their primary execution context.

### Verify

```bash
# Inspect the project manifest to identify the configured test runner and run all test suites covering API boundary handlers.
# Execute the repository static analysis and linting tools to verify route definitions and context usage compliance.
```

**Accept when:**
- All service boundary routes compile and pass automated test suites covering HTTP and protocol checking endpoints.
- Context metadata propagation operations complete without runtime type assertion failures or missing key errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>