---
version: 2
name: cross-cutting-error-handling-routines-capture-output-07f3
description: "Console API Logging: Error Handling Routines Capture Output Serialized"
metadata:
  actual:
    adr_id: 07f3cade-0211-4ac0-8d08-2899fc6b8ffe
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

# Console API Logging: Error Handling Routines Capture Output Serialized

Scope: applies to `apps/web/src/app/**`, `apps/web/src/components/**`, `apps/web/src/content/**`, `apps/web/src/lib/**`, `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for all server-side route handlers, background routines, and utility functions within the project.

### Rules

- **R-LOG-001** SHOULD: Error handling routines SHOULD capture and output serialized error details alongside descriptive context messages.

### Verify

```bash
# Discover the project test runner script and execute the test suite
# Discover the project static analysis and lint script and execute it
```

**Accept when:**
- All unit and integration tests execute successfully without unhandled logging errors.
- Static analysis checks pass with zero violations regarding prohibited logging statements.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>