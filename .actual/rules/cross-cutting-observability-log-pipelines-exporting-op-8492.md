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

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-LOG-001** SHOULD: Observability log pipelines exporting to OpenTelemetry endpoints SHOULD use @logtape/otel sinks.
- **R-LOG-002** MANDATORY: Initialize loggers at module scope using hierarchical category arrays to provide fine-grained filtering capabilities.
- **R-LOG-003** MANDATORY: Ensure error instances are normalized into dedicated metadata fields containing error name, message, and stack trace before emitting error log events.

### Verify

```bash
# Discover and execute the project static analysis script to verify logger imports
# Discover and execute the project test runner script to validate logging assertions
```

**Accept when:**
- Static analysis checks confirm that logging operations across all service modules import from @logtape/logtape and emit structured metadata objects.
- Automated test suites complete successfully and verify that contextual logging and error formatting comply with structured logging constraints.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated linting checks and peer reviews enforce structured metadata schemas, prevent unapproved console calls, and ensure that violations are remediated before code merges.
</enforcement>