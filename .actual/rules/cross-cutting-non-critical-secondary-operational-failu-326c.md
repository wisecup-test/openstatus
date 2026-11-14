---
version: 2
name: cross-cutting-non-critical-secondary-operational-failu-326c
description: "Console API for Non-Fatal Operational Warning Logging: Non Critical Secondary Operational Failures Such"
metadata:
  actual:
    adr_id: 326c6d80-2b52-4e6f-a108-046ae27f1ec0
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

# Console API for Non-Fatal Operational Warning Logging: Non Critical Secondary Operational Failures Such

Scope: applies to `packages/services/src/agent-tools/**`, `packages/services/src/auth/**`, `packages/services/src/external-service-component/**`, `packages/services/src/external-service/**`, `packages/services/src/integration/**`, `packages/services/src/page-subscriber/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-LOG-001** MUST_NOT: Non-critical secondary operational failures, such as notification delivery errors or individual record schema validation failures during collection retrieval, MUST NOT terminate the primary service execution flow.

### Verify

```bash
# Discover and execute repository static analysis and linting script
# Discover and run project test suite
```

**Accept when:**
- Static analysis passes with zero disallowed log levels and no unsanitized credential references in metadata payloads.
- All service unit and integration tests succeed without unhandled promise rejections during simulated secondary notification failures.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by continuous integration static analysis checks and peer code review verification.
</enforcement>