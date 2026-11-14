---
version: 2
name: cross-cutting-safe-fallback-execution-wrappers-log-5ac1
description: "Console API Tagged Prefix Logging for Error and Warning Diagnostics: Safe Fallback Execution Wrappers Log Specific"
metadata:
  actual:
    adr_id: 5ac12760-2040-4e6c-a7e5-bd1444aa50ee
    status: accepted
    candidate_group_id: 36d6268e-35a8-4f3e-88ed-9486d899106e
    evidence:
      source: deep
  scope:
    root: packages/tinybird
    paths:
      - scripts/**
      - src/**
---

# Console API Tagged Prefix Logging for Error and Warning Diagnostics: Safe Fallback Execution Wrappers Log Specific

Scope: applies to `packages/tinybird/scripts/**`, `packages/tinybird/src/**`.

These rules are ALWAYS ACTIVE for client integration modules managing external analytics connectivity, safe query execution, and operational data backfill/migration scripts interacting with primary datastores.

### Rules

- **R-LOG-001** MUST: Safe fallback execution wrappers MUST log the specific operation identifier and original error payload via console.error prior to returning safe empty fallback datasets to callers.
- **R-DISC-001** MUST (DISCOVERY POLICY): The consumer MUST derive all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-LOCK-001** MUST (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, execute in order: (1) Find dependency manifest, (2) Identify build tool, (3) Inspect repository lock/resolution artifact for exact resolved version, (4) Look up official docs for that exact version, (5) Confirm every API/class/function exists in that version, (6) Re-run steps 3-5 per dependency at point of use.
- **R-LOG-002** MUST: Standardize bracketed domain prefixes across all client diagnostic messages to maintain uniform log message parsing.
- **R-LOG-003** MUST: Ensure safe fallback routines attach the caught error object as a secondary parameter rather than concatenating it as a string to preserve stack traces.

### Verify

```bash
# Discover and execute repository static analysis and linting scripts from the dependency manifest
# Example derivation:
# npm run lint
# npm test
```

**Accept when:**
- Static analysis validates that all logging calls use designated console methods with standardized component prefix strings.
- Unit and integration tests verify that error fallback paths execute and emit diagnostic error messages without crashing.

<enforcement>
Claude Code MUST NOT skip or defer verification. Pull requests containing untagged console calls or swallowed exceptions without logging must be blocked until remediated.
</enforcement>