---
version: 2
name: cross-cutting-developers-not-emit-sensitive-credential-9b88
description: "Console API Logging: Developers Not Emit Sensitive Credentials Secrets"
metadata:
  actual:
    adr_id: 9b882fad-df55-4254-8ea5-45fe089f69b0
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

# Console API Logging: Developers Not Emit Sensitive Credentials Secrets

Scope: applies to `apps/web/src/app/**`, `apps/web/src/components/**`, `apps/web/src/content/**`, `apps/web/src/lib/**`, `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for all server-side route handlers, background routines, and utility functions within the project.

### Rules

- **R-LOG-001** MUST_NOT: Developers MUST NOT emit sensitive credentials, secrets, or raw authorization tokens into log statements.

### Verify

```bash
# Discover the project static analysis and lint script from the dependency manifest and execute it to check for prohibited logging patterns.
# Discover the repository test runner script from the project manifest and execute the test suite to verify log call execution.
```

**Accept when:**
- All unit and integration tests execute successfully without unhandled logging errors.
- Static analysis checks pass with zero violations regarding prohibited logging statements.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>