---
version: 2
name: cross-cutting-callers-bind-correlation-identifiers-rel-b40b
description: "Zerolog Context-Aware Structured Logging: Callers Bind Correlation Identifiers Relevant Execution"
metadata:
  actual:
    adr_id: b40bfb81-4b8f-4186-9918-0a6652727df7
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

# Zerolog Context-Aware Structured Logging: Callers Bind Correlation Identifiers Relevant Execution

Scope: applies to `apps/checker/checker/**`, `apps/checker/cmd/server/**`, `apps/checker/handlers/**`, `apps/checker/pkg/job/**`, `apps/checker/pkg/otel/**`, `apps/checker/pkg/tinybird/**`.

These rules are ALWAYS ACTIVE for execution flows, request handlers, protocol checkers, and background routines that accept a context, excluding pure utility transformations and early application initialization.

### Rules

- **R-ZEROLOG-001** SHOULD: Callers bind correlation identifiers and relevant execution metadata to the context before dispatching downstream tasks so that contextual logger instances automatically preserve operational traces.

### Verify

```bash
# Discover workspace static analysis and linting script from the project repository configuration and run it
# Discover automated test suite runner from the project repository configuration and run the test suite
```

**Accept when:**
- Static analysis and linting checks report zero violations of contextual logging conventions across all handlers, checkers, and service modules.
- Automated test suites complete with zero errors and confirm expected error logging behavior during simulated operational failures.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated linting, static analysis checks, and test suite execution.
</enforcement>