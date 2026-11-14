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

These rules are ALWAYS ACTIVE for service layer business logic handling non-fatal failures, background notification dispatches, data transformation discards, and authentication scope validation failures and authorization access denial reporting.

### Rules

- **R-LOG-001** MUST_NOT: Non-critical secondary operational failures, such as notification delivery errors or individual record schema validation failures during collection retrieval, MUST NOT terminate the primary service execution flow.
- **R-LOG-002** MANDATORY: Service handlers MUST ensure non-critical notification promises and async dispatches catch errors locally and forward them to warning emission rather than leaking unhandled rejections.
- **R-LOG-003** MANDATORY: Warning metadata payloads MUST be formatted as structured dictionary objects with explicit entity identifiers to assist log parsing systems.
- **R-LOG-004** MANDATORY: The consumer MUST derive all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-LOG-005** MANDATORY: Before writing code that uses a versioned library, execute the lock-version grounding steps in order (manifest inspection, build tool identification, lock file resolution lookup, official documentation fetch, API existence confirmation, and per-dependency re-running at point of use).

### Verify

```bash
# Discover and execute repository static analysis and linting script to validate warning log structure across service modules
# Discover and run the project test suite to verify non-fatal notification and schema validation failures emit warning logs
```

**Accept when:**
- Static analysis passes with zero disallowed log levels and no unsanitized credential references in metadata payloads.
- All service unit and integration tests succeed without unhandled promise rejections during simulated secondary notification failures.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>