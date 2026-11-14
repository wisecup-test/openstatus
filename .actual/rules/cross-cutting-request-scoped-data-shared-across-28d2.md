---
version: 2
name: cross-cutting-request-scoped-data-shared-across-28d2
description: "Adoption of Hono as Core Web Application Framework for Server Routing and Middleware: Request Scoped Data Shared Across Middleware"
metadata:
  actual:
    adr_id: 28d22a18-a3e4-46e8-b556-a2c52c98ecb8
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

# Adoption of Hono as Core Web Application Framework for Server Routing and Middleware: Request Scoped Data Shared Across Middleware

Scope: applies to `apps/server/src/libs/**`, `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HONO-001** MUST: Request-scoped data shared across middleware and endpoint handlers MUST be stored and retrieved via the framework context store rather than module-scoped or global mutable state.

### Verify

```bash
# Discover and execute the repository dependency audit script to verify all framework dependencies resolve to approved lock entries.
# Discover and execute the repository static analysis and type checking scripts to verify framework route and context typing compliance.
# Discover and execute the automated test runner to ensure all HTTP endpoint handlers and middleware pipelines pass validation.
```

**Accept when:**
- All server route handlers and middleware compile without type errors against the resolved framework context type contracts.
- Automated integration and unit test suites pass, verifying route dispatching and middleware chain execution.
- Static analysis confirms absence of unmanaged global request state in favor of framework context storage.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration builds fail upon detection of non-standard routing or direct transport socket manipulation, and pull requests are blocked until handlers are refactored into standard framework routes and middleware.
</enforcement>