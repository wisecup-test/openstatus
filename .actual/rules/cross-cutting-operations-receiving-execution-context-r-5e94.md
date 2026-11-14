---
version: 2
name: cross-cutting-operations-receiving-execution-context-r-5e94
description: "github.com/rs/zerolog Structured and Contextual Logging Adoption: Operations Receiving Execution Context Retrieve Contextual"
metadata:
  actual:
    adr_id: 5e94c7b7-1b04-4135-9217-faac61c9ef51
    status: accepted
    candidate_group_id: 69914c3b-8787-467a-9643-10591b9432bc
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - checker/**
      - pkg/logger/**
      - pkg/otel/**
---

# github.com/rs/zerolog Structured and Contextual Logging Adoption: Operations Receiving Execution Context Retrieve Contextual

Scope: applies to `apps/checker/checker/**`, `apps/checker/pkg/logger/**`, `apps/checker/pkg/otel/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-LOG-001** SHOULD: Operations receiving an execution context SHOULD retrieve contextual loggers via log.Ctx(ctx) to attach diagnostic attributes to error and event logs.

### Verify

```bash
# Discover and run the repository static analysis and linting scripts from project configuration
# Discover and execute the automated test runner to ensure all logging and telemetry contracts validate successfully
```

**Accept when:**
- Automated test and static analysis suites pass without unformatted standard stream log emissions.
- All service and telemetry logging invocations resolve through github.com/rs/zerolog interfaces.
- Dependency lock verification confirms that the resolved version matches the repository lock artifact.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>