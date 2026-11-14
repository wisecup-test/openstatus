---
version: 2
name: cross-cutting-log-statements-reporting-failures-chain-4c3d
description: "Zerolog Context-Aware Structured Logging: Log Statements Reporting Failures Chain Structured"
metadata:
  actual:
    adr_id: 4c3de7f4-1c25-466c-be31-22fc55f6c531
    status: accepted
    candidate_group_id: 47420a38-3bc2-4e9c-9bec-2e9a2aa8e81d
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - checker/**
      - cmd/server/**
      - handlers/**
      - pkg/job/**
      - pkg/otel/**
      - pkg/tinybird/**
---

# Zerolog Context-Aware Structured Logging: Log Statements Reporting Failures Chain Structured

Scope: applies to `apps/checker/checker/**`, `apps/checker/cmd/server/**`, `apps/checker/handlers/**`, `apps/checker/pkg/job/**`, `apps/checker/pkg/otel/**`, `apps/checker/pkg/tinybird/**`.

These rules are ALWAYS ACTIVE for execution flows, request handlers, protocol checkers, and background routines that accept a context.

### Rules

- **R-ZEROLOG-001** MUST: Log statements reporting failures MUST chain structured error attributes using `.Err(err)` and terminate with structured message dispatch via `.Msg(...)` on the contextual logger.

### Verify

```bash
# Discover workspace static analysis and linting script from repository configuration, then run it
# Discover automated test suite runner from repository configuration, then run the test suite
```

**Accept when:**
- Static analysis and linting checks report zero violations of contextual logging conventions across all handlers, checkers, and service modules.
- Automated test suites complete with zero errors and confirm expected error logging behavior during simulated operational failures.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>