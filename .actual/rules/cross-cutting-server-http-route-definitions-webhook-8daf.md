---
version: 2
name: cross-cutting-server-http-route-definitions-webhook-8daf
description: "Adoption of Hono as Core Web Application Framework for Server Routing and Middleware: Server Http Route Definitions Webhook Listeners"
metadata:
  actual:
    adr_id: 8dafcff1-feae-49bc-887b-04e3680a0803
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

# Adoption of Hono as Core Web Application Framework for Server Routing and Middleware: Server Http Route Definitions Webhook Listeners

Scope: applies to `apps/server/src/libs/**`, `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HONO-001** MUST: All server HTTP route definitions, webhook listeners, and request pipelines MUST be declared and mounted using Hono routing abstractions and context interfaces.

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
Claude Code MUST NOT skip or defer verification.
</enforcement>