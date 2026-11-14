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

These rules are ALWAYS ACTIVE for all service modules, background cron workflows, health checkers, and background tasks within the service execution boundaries.

### Rules

- **R-LOG-001** MUST: Service modules and background tasks MUST use @logtape/logtape for all application logging and telemetry event emission.
- **R-LOG-002** MANDATORY (Discovery Policy): The consumer MUST derive all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-LOG-003** MANDATORY (Lock-Version Grounding): Before writing code that uses a versioned library, execute in order: (1) Find dependency manifest, (2) Identify build tool, (3) Inspect repository lock/resolution artifact, (4) Look up official documentation for that exact version, (5) Confirm every API/class/function exists in that version, (6) Re-run steps 3-5 per dependency at point of use.
- **R-LOG-004** MUST: Initialize loggers at module scope using hierarchical category arrays to provide fine-grained filtering capabilities.
- **R-LOG-005** MUST: Ensure error instances are normalized into dedicated metadata fields containing error name, message, and stack trace before emitting error log events.

### Verify

```bash
# Discover and execute the project static analysis script to verify logger imports
# Discover and execute the project test runner script to validate structured logging and context propagation assertions
```

**Accept when:**
- Static analysis checks confirm that logging operations across all service modules import from @logtape/logtape and emit structured metadata objects.
- Automated test suites complete successfully and verify that contextual logging and error formatting comply with structured logging constraints.

<enforcement>
Verification is mandatory. Claude Code MUST NOT skip or defer verification.
</enforcement>