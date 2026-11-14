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

These rules are ALWAYS ACTIVE for service layer business logic handling non-fatal failures, background notification dispatches, data transformation discards, authentication scope validation failures, and authorization access denial reporting.

### Rules

- **R-LOG-001** SHOULD: Console warning log messages SHOULD use standardized bracketed subsystem prefixes when reporting domain-specific parsing or retrieval diagnostics.
- **R-LOG-002** MANDATORY: Service handlers MUST ensure non-critical notification promises and async dispatches catch errors locally and forward them to warning emission rather than leaking unhandled rejections.
- **R-LOG-003** MANDATORY: Warning metadata payloads MUST be formatted as structured dictionary objects with explicit entity identifiers to assist log parsing systems.
- **R-LOG-004** MANDATORY: The consumer MUST discover tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-LOG-005** MANDATORY (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, execute in order:
  1. Find the dependency manifest in the repo (declares ranges, not installed versions).
  2. Identify the build tool from the manifest.
  3. Inspect the repository lock or resolution artifact to determine the exact resolved version (authoritative).
  4. Look up official documentation, changelog, or public API reference for that exact version (no training-data recall).
  5. Confirm every API, class, or function you will call exists in that exact version's documentation.
  6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- **R-LOG-006** MANDATORY: Peer code reviews and automated static analysis rules MUST be enforced to sanitize logged metadata objects and prevent inadvertent logging of sensitive tenant information or authentication data.

### Verify

```bash
# Discover and execute the repository static analysis and linting script to validate warning log structure across service modules.
# Discover and run the project test suite to verify that non-fatal notification and schema validation failures emit warning logs without throwing unhandled exceptions.
```

**Accept when:**
- Static analysis passes with zero disallowed log levels and no unsanitized credential references in metadata payloads.
- All service unit and integration tests succeed without unhandled promise rejections during simulated secondary notification failures.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration static analysis and peer code review verification are mandatory.
</enforcement>