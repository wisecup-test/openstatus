---
version: 2
name: cross-cutting-log-statements-provide-structured-key-11c6
description: "@logtape/logtape Structured Logging Library: Log Statements Provide Structured Key Value"
metadata:
  actual:
    adr_id: 11c6daff-4277-47d2-b99c-fa24ff8cb8d5
    status: accepted
    candidate_group_id: aff3c2d6-39b7-48ae-8760-0f8041099307
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/**
---

# @logtape/logtape Structured Logging Library: Log Statements Provide Structured Key Value

Scope: applies to `apps/workflows/src/**`.

These rules are ALWAYS ACTIVE for all service handlers, cron tasks, health checkers, and background workflows within the service execution boundaries.

### Rules

- **R-LOG-001** MUST: Log statements MUST provide structured key-value property bags for dynamic event attributes rather than concatenating values into plain text strings.

### Verify

```bash
# Discover and execute the project static analysis script from the dependency manifest to verify logger imports
# Discover and execute the project test runner script to validate structured logging assertions
```

**Accept when:**
- Static analysis checks confirm that logging operations across all service modules import the designated structured logging library and emit structured metadata objects.
- Automated test suites complete successfully and verify that contextual logging and error formatting comply with structured logging constraints.

<enforcement>
Claude Code MUST NOT skip or defer verification. All code changes adhering to this rule must be verified via the prescribed project scripts.
</enforcement>