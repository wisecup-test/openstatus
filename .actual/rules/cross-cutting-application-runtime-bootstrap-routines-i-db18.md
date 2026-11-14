---
version: 2
name: cross-cutting-application-runtime-bootstrap-routines-i-db18
description: "github.com/rs/zerolog Structured and Contextual Logging Adoption: Application Runtime Bootstrap Routines Initialize Logging"
metadata:
  actual:
    adr_id: db18d667-403c-4422-a57f-aa96ff622ec2
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

# github.com/rs/zerolog Structured and Contextual Logging Adoption: Application Runtime Bootstrap Routines Initialize Logging

Scope: applies to `apps/checker/checker/**`, `apps/checker/pkg/logger/**`, `apps/checker/pkg/otel/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-LOG-001** MUST: Application runtime bootstrap routines MUST initialize logging behavior through the centralized Configure contract prior to starting service operations.

### Verify

```bash
# Discover and execute the project's static analysis and linting scripts
# Discover and execute the automated test runner to verify logging contracts
```

**Accept when:**
- Automated test and static analysis suites pass without unformatted standard stream log emissions.
- All service and telemetry logging invocations resolve through github.com/rs/zerolog interfaces.
- Dependency lock verification confirms that the resolved version matches the repository lock artifact.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>