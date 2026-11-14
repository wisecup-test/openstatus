---
version: 2
name: cross-cutting-context-keys-used-set-get-3091
description: "Gin Framework Request-Scoped Context Storage: Context Keys Used Set Get Defined"
metadata:
  actual:
    adr_id: 30913dc6-8d87-494c-8988-0197b1571c52
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

# Gin Framework Request-Scoped Context Storage: Context Keys Used Set Get Defined

Scope: applies to `apps/checker/checker/**`, `apps/checker/cmd/private/**`, `apps/checker/cmd/server/**`, `apps/checker/handlers/**`, `apps/checker/pkg/tinybird/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-GIN-001** SHOULD: Context keys used with c.Set and c.Get SHOULD be defined as centralized package constants rather than untyped raw string literals.

### Verify

```bash
# Discover and run repository static analysis, linting scripts, and the project test suite
# to verify type assertions and context key usages.
```

**Accept when:**
- All unit and integration tests covering request handlers pass without context retrieval failures or nil pointer panics
- Static analysis checks pass with zero unhandled type assertion warnings on context values

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests containing unhandled context type assertions or string literal keys will be blocked.
</enforcement>