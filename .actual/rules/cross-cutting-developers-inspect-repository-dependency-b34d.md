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

These rules are ALWAYS ACTIVE for execution flows, request handlers, protocol checkers, and background routines that accept a context.

### Rules

- **R-ZER-001** MUST: Developers MUST inspect the repository dependency lock artifact to determine the exact resolved version of `github.com/rs/zerolog` and verify API compatibility against that exact version documentation prior to invoking logging methods.

### Verify

```bash
# Discover and run workspace static analysis / linting script
# Discover and run the automated test suite runner
```

**Accept when:**
- Static analysis and linting checks report zero violations of contextual logging conventions across all handlers, checkers, and service modules.
- Automated test suites complete with zero errors and confirm expected error logging behavior during simulated operational failures.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification by automated linting, static analysis, and peer review is mandatory.
</enforcement>