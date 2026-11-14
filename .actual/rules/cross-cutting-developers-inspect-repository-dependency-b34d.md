---
version: 2
name: cross-cutting-developers-inspect-repository-dependency-b34d
description: "Zerolog Context-Aware Structured Logging: Developers Inspect Repository Dependency Lock Artifact"
metadata:
  actual:
    adr_id: b34d1cbc-c652-4a50-89af-6ea783157d97
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

# Zerolog Context-Aware Structured Logging: Developers Inspect Repository Dependency Lock Artifact

Scope: applies to `apps/checker/checker/**`, `apps/checker/cmd/server/**`, `apps/checker/handlers/**`, `apps/checker/pkg/job/**`, `apps/checker/pkg/otel/**`, `apps/checker/pkg/tinybird/**`.

These rules are ALWAYS ACTIVE for execution flows, request handlers, protocol checkers, background routines, and services that accept a context.

### Rules

- **R-LOG-001** MUST: Inspect the repository dependency lock artifact to determine the exact resolved version of `github.com/rs/zerolog` and verify API compatibility against that exact version documentation prior to invoking logging methods.
- **R-LOG-002** MUST: Extract loggers from contexts using context logging utilities (such as `log.Ctx(ctx)`) in execution flows, request handlers, protocol checkers, and background routines that accept a context, rather than using global output functions or passing loggers explicitly.
- **R-LOG-003** MUST: Initialize root loggers with required baseline attributes during service startup and attach them to parent contexts prior to dispatching operational handlers.
- **R-LOG-004** MUST: Preserve the original error instance by passing it directly to the structured error logging method when handling operational errors within loops or retry callbacks.

### Verify

```bash
# Discover and run the workspace static analysis, linting, and automated test suite scripts
# as defined in the project repository configuration.
```

**Accept when:**
- Static analysis and linting checks report zero violations of contextual logging conventions across all handlers, checkers, and service modules.
- Automated test suites complete with zero errors and confirm expected error logging behavior during simulated operational failures.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>