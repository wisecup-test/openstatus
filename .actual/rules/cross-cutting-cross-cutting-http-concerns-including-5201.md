---
version: 2
name: cross-cutting-cross-cutting-http-concerns-including-5201
description: "Adoption of Hono as Core Web Application Framework for Server Routing and Middleware: Cross Cutting Http Concerns Including Authentication"
metadata:
  actual:
    adr_id: 52014e2a-c4a3-48d3-8e6c-37d96c01d9b5
    status: accepted
    candidate_group_id: ab635fa0-3163-43ac-b634-a36e9733e3a2
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/libs/**
      - src/routes/**
---

# Adoption of Hono as Core Web Application Framework for Server Routing and Middleware: Cross Cutting Http Concerns Including Authentication

Scope: applies to `apps/server/src/libs/**`, `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HONO-001** MUST: Cross-cutting HTTP concerns including authentication, error formatting, cross-origin resource sharing, and performance metrics MUST be implemented as composable framework middleware functions.

### Verify

```bash
# Discover and execute repository dependency audit script
# Discover and execute repository static analysis and type checking scripts
# Discover and execute automated test runner
```

**Accept when:**
- All server route handlers and middleware compile without type errors against the resolved framework context type contracts.
- Automated integration and unit test suites pass, verifying route dispatching and middleware chain execution.
- Static analysis confirms absence of unmanaged global request state in favor of framework context storage.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>