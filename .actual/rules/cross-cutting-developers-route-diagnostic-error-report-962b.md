---
version: 2
name: cross-cutting-developers-route-diagnostic-error-report-962b
description: "Console API Logging: Developers Route Diagnostic Error Reporting Through"
metadata:
  actual:
    adr_id: 962b85bf-2a72-4d8c-8e9f-543a55ae1ba7
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

# Console API Logging: Developers Route Diagnostic Error Reporting Through

Scope: applies to `apps/web/src/app/**`, `apps/web/src/components/**`, `apps/web/src/content/**`, `apps/web/src/lib/**`, `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for all server-side route handlers, background routines, and utility functions within the project.

### Rules

- **R-CON-001** MUST: Route diagnostic and error reporting through standardized runtime console methods until a formal structured logging client is adopted.

### Verify

```bash
# Discover and execute the repository test runner script from the project manifest
# Discover and execute the project static analysis and lint script from the dependency manifest
```

**Accept when:**
- All unit and integration tests execute successfully without unhandled logging errors.
- Static analysis checks pass with zero violations regarding prohibited logging statements.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>