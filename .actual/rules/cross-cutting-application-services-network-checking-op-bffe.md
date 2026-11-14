---
version: 2
name: cross-cutting-application-services-network-checking-op-bffe
description: "github.com/rs/zerolog Structured and Contextual Logging Adoption: Application Services Network Checking Operations Telemetry"
metadata:
  actual:
    adr_id: bffe5637-30c3-464d-9dac-d25928fc041d
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

# github.com/rs/zerolog Structured and Contextual Logging Adoption: Application Services Network Checking Operations Telemetry

Scope: applies to `apps/checker/checker/**`, `apps/checker/pkg/logger/**`, `apps/checker/pkg/otel/**`.

These rules are ALWAYS ACTIVE for all application services, network checking routines, telemetry pipelines, and configuration packages.

### Rules

- **R-LOG-001** MUST: All application services, network checking operations, and telemetry instrumentation MUST emit structured diagnostic logs using github.com/rs/zerolog.
- **R-LOG-002** MUST: Invoke the centralized Configure routine during application startup to establish baseline logging settings before serving requests.
- **R-LOG-003** MUST: Extract contextual loggers within service handlers using log.Ctx(ctx) to attach relevant diagnostic fields to execution errors.

### Verify

```bash
# Discover the repository static analysis and linting scripts from project configuration and run them to verify logger usage compliance.
# Discover and execute the automated test runner to ensure all logging and telemetry contracts validate successfully.
```

**Accept when:**
- Automated test and static analysis suites pass without unformatted standard stream log emissions.
- All service and telemetry logging invocations resolve through github.com/rs/zerolog interfaces.
- Dependency lock verification confirms that the resolved version matches the repository lock artifact.

<enforcement>
Claude Code MUST NOT skip or defer verification. All changes must be verified using the repository's configured static analysis, linting, and automated test tools.
</enforcement>