---
version: 2
name: cross-cutting-background-execution-failures-occurring-c175
description: "Context Package Asynchronous Lifecycle Isolation via context.WithoutCancel: Background Execution Failures Occurring Within Detached"
metadata:
  actual:
    adr_id: c17541c0-34bc-44bf-b231-17e29e270f5e
    status: accepted
    candidate_group_id: 1700c304-66aa-4383-b313-bb2df6cb6fc3
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - cmd/server/**
      - internal/server/**
---

# Context Package Asynchronous Lifecycle Isolation via context.WithoutCancel: Background Execution Failures Occurring Within Detached

Scope: applies to `apps/private-location/cmd/server/**`, `apps/private-location/internal/server/**`.

These rules are ALWAYS ACTIVE for all asynchronous downstream status reporting, telemetry forwarding, and audit dispatch triggered within request handlers, and background operations requiring access to parent context values that must survive parent cancellation.

### Rules

- **R-BACK-001** SHOULD: Background execution failures occurring within detached context routines SHOULD be recorded to structured logging destinations with descriptive contextual attributes.
- **R-BACK-002** MANDATORY: The consumer MUST derive all tool names, file names, commands, package managers, and version numbers from the project repository following the discovery policy.
- **R-BACK-003** MANDATORY: Before writing code that uses a versioned library, execute in order: find dependency manifest, identify build tool, inspect repository lock or resolution artifact for exact resolved version, look up official documentation for that exact version, confirm every API/class/function exists in that version's documentation, and re-run steps per dependency at point of use.
- **R-BACK-004** MANDATORY: Ensure all detached goroutines handle errors locally and record failures to structured logging.
- **R-BACK-005** MANDATORY: Coordinate server shutdown handlers to allow active bounded detached operations sufficient window to complete.

### Verify

```bash
echo "Discover and execute the repository test runner defined in the project build configuration"
echo "Execute static analysis and linting checks discovered from the repository manifest"
```

**Accept when:**
- Detached context routines execute to completion when parent request contexts cancel prematurely.
- All derived detached contexts enforce bounded timeouts and invoke deferred cancellation cleanly.
- Repository test suites and static analysis checks pass without errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated static analysis checks in the continuous integration pipeline and peer code review of server request handlers and concurrency routines.
</enforcement>