---
version: 2
name: cross-cutting-handlers-retrieving-values-via-get-5e38
description: "Gin Framework Request-Scoped Context Storage: Handlers Retrieving Values Via Get Perform"
metadata:
  actual:
    adr_id: 5e38edab-c76c-49bb-b8b3-6224dc338ff8
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

# Gin Framework Request-Scoped Context Storage: Handlers Retrieving Values Via Get Perform

Scope: applies to `apps/checker/checker/**`, `apps/checker/cmd/private/**`, `apps/checker/cmd/server/**`, `apps/checker/handlers/**`, `apps/checker/pkg/tinybird/**`.

These rules are ALWAYS ACTIVE for HTTP server request routing, middleware layers, protocol checker handlers receiving framework request contexts, and transient per-request telemetry and event state propagation.

### Rules

- **R-GIN-001** MUST: Handlers retrieving values via c.Get MUST perform explicit type assertions and handle missing or invalid type conditions to prevent unhandled runtime panics.

### Verify

```bash
# Discover and run the repository static analysis and linting scripts to verify type assertions and context key usages.
# Discover and run the project test suite to validate request handler context propagation and error handling.
```

**Accept when:**
- All unit and integration tests covering request handlers pass without context retrieval failures or nil pointer panics
- Static analysis checks pass with zero unhandled type assertion warnings on context values

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration pipeline running test suites and static analysis linters, and peer code review verifying safe context extraction and adherence to centralized key constants.
</enforcement>