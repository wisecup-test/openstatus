---
version: 2
name: cross-cutting-components-not-invoke-uncontextualized-g-2754
description: "Zerolog Context-Aware Structured Logging: Components Not Invoke Uncontextualized Global Logger"
metadata:
  actual:
    adr_id: 2754f22a-9cfb-4b06-a375-35dc8ec792da
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

# Zerolog Context-Aware Structured Logging: Components Not Invoke Uncontextualized Global Logger

Scope: applies to `apps/checker/checker/**`, `apps/checker/cmd/server/**`, `apps/checker/handlers/**`, `apps/checker/pkg/job/**`, `apps/checker/pkg/otel/**`, `apps/checker/pkg/tinybird/**`.

These rules are ALWAYS ACTIVE for execution flows, request handlers, protocol checkers, and background routines that accept a context.

### Rules

- **R-ZLOG-001** MUST_NOT: Components MUST_NOT invoke uncontextualized global logger instances or format unparsed error strings into raw text output when a contextual logger can be derived.

### Verify

```bash
# Discover the workspace static analysis and linting script from the project repository configuration, then run it.
# Discover the automated test suite runner from the project repository configuration, then run the test suite.
```

**Accept when:**
- Static analysis and linting checks report zero violations of contextual logging conventions across all handlers, checkers, and service modules.
- Automated test suites complete with zero errors and confirm expected error logging behavior during simulated operational failures.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>