---
version: 2
name: cross-cutting-console-warning-log-messages-use-14d2
description: "Console API for Non-Fatal Operational Warning Logging: Console Warning Log Messages Use Standardized"
metadata:
  actual:
    adr_id: 14d256ed-fb7b-492b-97ec-352c15454207
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

# Console API for Non-Fatal Operational Warning Logging: Console Warning Log Messages Use Standardized

Scope: applies to `packages/services/src/agent-tools/**`, `packages/services/src/auth/**`, `packages/services/src/external-service-component/**`, `packages/services/src/external-service/**`, `packages/services/src/integration/**`, `packages/services/src/page-subscriber/**`.

These rules are ALWAYS ACTIVE for service layer business logic handling non-fatal failures, background notification dispatches, data transformation discards, and authentication scope validation failures.

### Rules

- **R-CON-001** SHOULD: Console warning log messages SHOULD use standardized bracketed subsystem prefixes when reporting domain-specific parsing or retrieval diagnostics.

### Verify

```bash
# Discover and execute the repository static analysis and linting script to validate warning log structure across service modules.
# Discover and run the project test suite to verify that non-fatal notification and schema validation failures emit warning logs without throwing unhandled exceptions.
```

**Accept when:**
- Static analysis passes with zero disallowed log levels and no unsanitized credential references in metadata payloads.
- All service unit and integration tests succeed without unhandled promise rejections during simulated secondary notification failures.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration static analysis checks and peer code reviews enforce compliance.
</enforcement>