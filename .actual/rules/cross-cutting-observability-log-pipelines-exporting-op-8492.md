---
version: 2
name: cross-cutting-observability-log-pipelines-exporting-op-8492
description: "@logtape/logtape Structured Logging Library: Observability Log Pipelines Exporting Opentelemetry Endpoints"
metadata:
  actual:
    adr_id: 84925605-e32d-4192-990d-bcfa789eb1e1
    status: accepted
    candidate_group_id: aff3c2d6-39b7-48ae-8760-0f8041099307
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/**
---

# @logtape/logtape Structured Logging Library: Observability Log Pipelines Exporting Opentelemetry Endpoints

Scope: applies to `apps/workflows/src/**`.

These rules are ALWAYS ACTIVE for all service handlers, cron tasks, health checkers, and background workflows within the service execution boundaries.

### Rules

- **R-LOG-001** SHOULD: Observability log pipelines exporting to OpenTelemetry endpoints SHOULD use @logtape/otel sinks.

### Verify

```bash
# Discover and execute the project static analysis script to verify logger imports
# Discover and execute the project test runner script to validate structured logging and context propagation assertions
```

**Accept when:**
- Static analysis checks confirm that logging operations across all service modules import from @logtape/logtape and emit structured metadata objects.
- Automated test suites complete successfully and verify that contextual logging and error formatting comply with structured logging constraints.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>