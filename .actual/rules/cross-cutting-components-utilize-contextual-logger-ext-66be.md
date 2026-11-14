---
version: 2
name: cross-cutting-components-utilize-contextual-logger-ext-66be
description: "Zerolog Context-Aware Structured Logging: Components Utilize Contextual Logger Extraction Via"
metadata:
  actual:
    adr_id: 66bedf72-8262-49e0-9284-3f93e1ff39e8
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

# Zerolog Context-Aware Structured Logging: Components Utilize Contextual Logger Extraction Via

Scope: applies to `apps/checker/checker/**`, `apps/checker/cmd/server/**`, `apps/checker/handlers/**`, `apps/checker/pkg/job/**`, `apps/checker/pkg/otel/**`, `apps/checker/pkg/tinybird/**`.

These rules are ALWAYS ACTIVE for execution flows, request handlers, protocol checkers, and background routines that accept a context.

### Rules

- **R-LOG-001** MUST: Components MUST utilize contextual logger extraction via `log.Ctx(ctx)` from `github.com/rs/zerolog/log` to record events and errors whenever an execution context is present.

### Verify

```bash
# Discover the workspace static analysis and linting script from the project repository configuration, then run it:
# e.g., go vet ./... or custom linter

# Run automated tests to verify logging behavior during execution flows:
go test ./...
```

**Accept when:**
- Static analysis and linting checks report zero violations of contextual logging conventions across all handlers, checkers, and service modules.
- Automated test suites complete with zero errors and confirm expected error logging behavior during simulated operational failures.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>