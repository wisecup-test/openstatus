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

These rules are ALWAYS ACTIVE for execution flows, request handlers, protocol checkers, and background routines that accept a context, excluding pure utility transformations and early application startup initialization prior to context creation.

### Rules

- **R-ZEROLOG-001** SHOULD: Callers bind correlation identifiers and relevant execution metadata to the context before dispatching downstream tasks so that contextual logger instances automatically preserve operational traces.
- **R-ZEROLOG-002** MANDATORY: Initialize root loggers with required baseline attributes during service startup and attach them to parent contexts prior to dispatching operational handlers.
- **R-ZEROLOG-003** MANDATORY: When handling operational errors within loops or retry callbacks, preserve the original error instance by passing it directly to the structured error logging method.

### Verify

```bash
# Discover the workspace static analysis and linting script from the project repository configuration, then run it
# Discover the automated test suite runner from the project repository configuration, then run the test suite
```

**Accept when:**
- Static analysis and linting checks report zero violations of contextual logging conventions across all handlers, checkers, and service modules.
- Automated test suites complete with zero errors and confirm expected error logging behavior during simulated operational failures.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests containing uncontextualized log invocations in context-aware paths are blocked from merging.
</enforcement>