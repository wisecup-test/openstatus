---
version: 2
name: cross-cutting-service-modules-not-use-native-f305
description: "@logtape/logtape Structured Logging Library: Service Modules Not Use Native Runtime"
metadata:
  actual:
    adr_id: f305264d-bf72-4481-b949-24c5c9f4f35e
    status: accepted
    candidate_group_id: aff3c2d6-39b7-48ae-8760-0f8041099307
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/**
---

# @logtape/logtape Structured Logging Library: Service Modules Not Use Native Runtime

Scope: applies to `apps/workflows/src/**`.

These rules are ALWAYS ACTIVE for all service handlers, cron tasks, health checkers, and background workflows within the service execution boundaries.

### Rules

- **R-LOG-001** MUST_NOT: Service modules MUST NOT use native runtime console printing methods for production logging.

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