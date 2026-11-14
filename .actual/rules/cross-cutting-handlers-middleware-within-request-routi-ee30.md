---
version: 2
name: cross-cutting-handlers-middleware-within-request-routi-ee30
description: "Gin Framework Request-Scoped Context Storage: Handlers Middleware Within Request Routing Pipeline"
metadata:
  actual:
    adr_id: ee3075a3-374d-4fe4-9130-a047836b0bc5
    status: accepted
    candidate_group_id: 3cda31a0-dcc3-403b-8349-6d2b958c7ca4
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - checker/**
      - cmd/private/**
      - cmd/server/**
      - handlers/**
      - pkg/tinybird/**
---

# Gin Framework Request-Scoped Context Storage: Handlers Middleware Within Request Routing Pipeline

Scope: applies to `apps/checker/checker/**`, `apps/checker/cmd/private/**`, `apps/checker/cmd/server/**`, `apps/checker/handlers/**`, `apps/checker/pkg/tinybird/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-GIN-001** MUST: Handlers and middleware within the request routing pipeline MUST use the adopted web framework context methods c.Set and c.Get for managing and propagating transient, request-scoped event state and tracing identifiers.

### Verify

```bash
# Discover and run the repository static analysis and linting scripts to verify type assertions and context key usages.
# Discover and run the project test suite to validate request handler context propagation and error handling.
```

**Accept when:**
- All unit and integration tests covering request handlers pass without context retrieval failures or nil pointer panics
- Static analysis checks pass with zero unhandled type assertion warnings on context values

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>