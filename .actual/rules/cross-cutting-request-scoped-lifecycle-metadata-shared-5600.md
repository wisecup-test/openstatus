---
version: 2
name: cross-cutting-request-scoped-lifecycle-metadata-shared-5600
description: "Gin Framework Service Boundary Definition and Context Propagation: Request Scoped Lifecycle Metadata Shared Event"
metadata:
  actual:
    adr_id: 56002a43-fbd1-4e9d-8e58-54a7ab5581f9
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

# Gin Framework Service Boundary Definition and Context Propagation: Request Scoped Lifecycle Metadata Shared Event

Scope: applies to `apps/checker/checker/**`, `apps/checker/cmd/server/**`, `apps/checker/handlers/**`, `apps/checker/pkg/otel/**`, `apps/checker/pkg/tinybird/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-GIN-001** MUST: Request-scoped lifecycle metadata and shared event state passed across middleware and boundary handlers MUST be stored and retrieved using context Set and Get methods.

### Verify

```bash
# Inspect the project manifest to identify the configured test runner and run all test suites covering API boundary handlers.
# Execute the repository static analysis and linting tools to verify route definitions and context usage compliance.
```

**Accept when:**
- All service boundary routes compile and pass automated test suites covering HTTP and protocol checking endpoints.
- Context metadata propagation operations complete without runtime type assertion failures or missing key errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration checks and mandatory peer code review.
</enforcement>