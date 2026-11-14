---
version: 2
name: cross-cutting-mutable-request-context-state-not-7377
description: "Gin Framework Request-Scoped Context Storage: Mutable Request Context State Not Accessed"
metadata:
  actual:
    adr_id: 7377a030-fe14-46d0-b678-50bde0641d47
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

# Gin Framework Request-Scoped Context Storage: Mutable Request Context State Not Accessed

Scope: applies to `apps/checker/checker/**`, `apps/checker/cmd/private/**`, `apps/checker/cmd/server/**`, `apps/checker/handlers/**`, `apps/checker/pkg/tinybird/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-GIN-001** SHOULD_NOT: Mutable request context state SHOULD_NOT be accessed concurrently across multiple uncoordinated background routines without synchronization.

### Verify

```bash
# Discover and run the repository static analysis and linting scripts to verify type assertions and context key usages.
# Discover and run the project test suite to validate request handler context propagation and error handling.
```

**Accept when:**
- All unit and integration tests covering request handlers pass without context retrieval failures or nil pointer panics
- Static analysis checks pass with zero unhandled type assertion warnings on context values

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration pipeline running test suites, static analysis linters, and peer code reviews verify safe context extraction and adherence to centralized key constants.
</enforcement>