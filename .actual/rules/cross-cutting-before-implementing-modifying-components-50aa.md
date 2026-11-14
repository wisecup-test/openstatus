---
version: 2
name: cross-cutting-before-implementing-modifying-components-50aa
description: "Gin Framework Request-Scoped Context Storage: Before Implementing Modifying Components That Interact"
metadata:
  actual:
    adr_id: 50aae829-96fc-470d-b58d-96ba163074a3
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

# Gin Framework Request-Scoped Context Storage: Before Implementing Modifying Components That Interact

Scope: applies to `apps/checker/checker/**`, `apps/checker/cmd/private/**`, `apps/checker/cmd/server/**`, `apps/checker/handlers/**`, `apps/checker/pkg/tinybird/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-GIN-001** MUST: Before implementing or modifying components that interact with the web framework context API, developers MUST discover the dependency manifest and authoritative resolution lock artifact in the project repository to verify the exact resolved version and its published API contracts.

### Verify

```bash
# Discover and run the repository static analysis and linting scripts to verify type assertions and context key usages
# Discover and run the project test suite to validate request handler context propagation and error handling
```

**Accept when:**
- All unit and integration tests covering request handlers pass without context retrieval failures or nil pointer panics
- Static analysis checks pass with zero unhandled type assertion warnings on context values

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>