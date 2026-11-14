---
version: 2
name: cross-cutting-service-modules-background-tasks-use-f517
description: "@logtape/logtape Structured Logging Library: Service Modules Background Tasks Use Logtape"
metadata:
  actual:
    adr_id: f517bbcf-f628-4d3f-9b51-79c8cf152851
    status: accepted
    candidate_group_id: aff3c2d6-39b7-48ae-8760-0f8041099307
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/**
---

# @logtape/logtape Structured Logging Library: Service Modules Background Tasks Use Logtape

Scope: applies to `apps/workflows/src/**`.

These rules are ALWAYS ACTIVE for all service modules, background cron tasks, health checkers, and background workflows within the service execution boundaries.

### Rules

- **R-LOG-001** MUST: Service modules and background tasks MUST use @logtape/logtape for all application logging and telemetry event emission.
- **R-LOG-002** MANDATORY: Prior to writing code using a versioned library, execute the lock-version grounding sequence (find dependency manifest, identify build tool, inspect lock/resolution artifact for the exact resolved version, look up official documentation for that exact version, confirm APIs exist, and re-run per dependency at point of use).
- **R-LOG-003** MANDATORY: Initialize loggers at module scope using hierarchical category arrays to provide fine-grained filtering capabilities.
- **R-LOG-004** MANDATORY: Ensure error instances are normalized into dedicated metadata fields containing error name, message, and stack trace before emitting error log events.

### Verify

```bash
# Discover and execute the project static analysis script to verify logger imports
# (Discover the static analysis script from the dependency manifest and run it)
# Discover and execute the project test runner script to validate structured logging and context propagation assertions
```

**Accept when:**
- Static analysis checks confirm that logging operations across all service modules import from @logtape/logtape and emit structured metadata objects.
- Automated test suites complete successfully and verify that contextual logging and error formatting comply with structured logging constraints.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification by automated continuous integration linting checks and pull request review is mandatory. Violations must be remediated to use @logtape/logtape structured log methods before code merges.
</enforcement>