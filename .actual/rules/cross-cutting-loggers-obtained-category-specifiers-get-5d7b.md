---
version: 2
name: cross-cutting-loggers-obtained-category-specifiers-get-5d7b
description: "@logtape/logtape Structured Logging Library: Loggers Obtained Category Specifiers Getlogger Enable"
metadata:
  actual:
    adr_id: 5d7b96e7-d8e8-436a-bb06-54dba90be8ee
    status: accepted
    candidate_group_id: aff3c2d6-39b7-48ae-8760-0f8041099307
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/**
---

# @logtape/logtape Structured Logging Library: Loggers Obtained Category Specifiers Getlogger Enable

Scope: applies to `apps/workflows/src/**`.

These rules are ALWAYS ACTIVE for all service handlers, cron tasks, health checkers, and background workflows within the service execution boundaries.

### Rules

- **R-LOG-001** SHOULD: Loggers SHOULD be obtained with category specifiers using getLogger to enable hierarchical log filtering.
- **R-LOG-002** MANDATORY: Initialize loggers at module scope using hierarchical category arrays to provide fine-grained filtering capabilities.
- **R-LOG-003** MANDATORY: Ensure error instances are normalized into dedicated metadata fields containing error name, message, and stack trace before emitting error log events.

### Verify

```bash
# Discover the repository static analysis script from the dependency manifest and execute it to verify that all logger imports reference @logtape/logtape.
# Discover and execute the project test runner script to validate that structured logging and context propagation assertions pass.
```

**Accept when:**
- Static analysis checks confirm that logging operations across all service modules import from @logtape/logtape and emit structured metadata objects.
- Automated test suites complete successfully and verify that contextual logging and error formatting comply with structured logging constraints.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>