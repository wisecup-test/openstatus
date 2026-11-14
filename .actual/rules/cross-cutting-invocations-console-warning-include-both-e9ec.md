---
version: 2
name: cross-cutting-invocations-console-warning-include-both-e9ec
description: "Console API for Non-Fatal Operational Warning Logging: Invocations Console Warning Include Both Explicit"
metadata:
  actual:
    adr_id: e9ecd2d8-f0f9-4f2d-9ddf-9971597b837c
    status: accepted
    candidate_group_id: ed557730-6a2b-4f7c-9bb3-e3efb7e036aa
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/agent-tools/**
      - src/auth/**
      - src/external-service-component/**
      - src/external-service/**
      - src/integration/**
      - src/page-subscriber/**
---

# Console API for Non-Fatal Operational Warning Logging: Invocations Console Warning Include Both Explicit

Scope: applies to `packages/services/src/agent-tools/**`, `packages/services/src/auth/**`, `packages/services/src/external-service-component/**`, `packages/services/src/external-service/**`, `packages/services/src/integration/**`, `packages/services/src/page-subscriber/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-WARN-001** MUST: Invocations of the console warning API MUST include both an explicit diagnostic message string and a structured contextual object containing identifying entity keys or error causes.

### Verify

```bash
# Discover and execute the repository static analysis and linting script to validate warning log structure across service modules.
# Discover and run the project test suite to verify that non-fatal notification and schema validation failures emit warning logs without throwing unhandled exceptions.
```

**Accept when:**
- Static analysis passes with zero disallowed log levels and no unsanitized credential references in metadata payloads.
- All service unit and integration tests succeed without unhandled promise rejections during simulated secondary notification failures.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>