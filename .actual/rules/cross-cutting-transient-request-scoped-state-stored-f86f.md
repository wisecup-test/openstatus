---
version: 2
name: cross-cutting-transient-request-scoped-state-stored-f86f
description: "Gin Framework Request-Scoped Context Storage: Transient Request Scoped State Stored Framework"
metadata:
  actual:
    adr_id: f86f210a-9473-4ada-85b8-84e6294b1fca
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

# Gin Framework Request-Scoped Context Storage: Transient Request Scoped State Stored Framework

Scope: applies to `apps/checker/checker/**`, `apps/checker/cmd/private/**`, `apps/checker/cmd/server/**`, `apps/checker/handlers/**`, `apps/checker/pkg/tinybird/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-GIN-001** MUST_NOT: Transient request-scoped state stored in the framework context via c.Set MUST_NOT be used as a substitute for persistent storage across independent requests or outside the active request lifecycle.

### Verify

```bash
# Discover and run repository static analysis, linters, and test suite
# (Commands to be discovered from repository configuration)
```

**Accept when:**
- All unit and integration tests covering request handlers pass without context retrieval failures or nil pointer panics
- Static analysis checks pass with zero unhandled type assertion warnings on context values

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>