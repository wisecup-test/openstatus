---
version: 2
name: cross-cutting-log-statements-include-bracketed-subsyst-a569
description: "Console API Logging: Log Statements Include Bracketed Subsystem Prefix"
metadata:
  actual:
    adr_id: a5695982-d528-4b5b-b2b0-053c3dc9b8e9
    status: accepted
    candidate_group_id: 0174bab9-9040-4ba3-9c11-4dcd869b8464
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
      - src/components/**
      - src/content/**
      - src/lib/**
      - src/trpc/**
---

# Console API Logging: Log Statements Include Bracketed Subsystem Prefix

Scope: applies to `apps/web/src/app/**`, `apps/web/src/components/**`, `apps/web/src/content/**`, `apps/web/src/lib/**`, `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for all server-side route handlers, background routines, and utility functions within the project.

### Rules

- **R-LOG-001** SHOULD: Log statements SHOULD include a bracketed subsystem prefix to identify the emitting component or module in unstructured log outputs.

### Verify

```bash
# Discover and execute the project test runner script to verify log call execution
npm test

# Discover and execute the project static analysis and lint script to check for logging patterns
npm run lint
```

**Accept when:**
- All unit and integration tests execute successfully without unhandled logging errors.
- Static analysis checks pass with zero violations regarding prohibited logging statements.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests and code modifications must adhere to subsystem prefixing and automated static analysis checks.
</enforcement>