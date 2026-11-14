---
version: 2
name: cross-cutting-route-definitions-attach-authorization-m-f9a4
description: "Adopt Hono Framework for Workflow Service Routing and Middleware: Route Definitions Attach Authorization Middleware Validate"
metadata:
  actual:
    adr_id: f9a4faaa-217e-4f0f-a2a8-2e904fe0a346
    status: accepted
    candidate_group_id: 21dd5eee-ded5-43d1-9312-6f96f5101826
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/checker/**
      - src/cron/**
      - src/incident/**
---

# Adopt Hono Framework for Workflow Service Routing and Middleware: Route Definitions Attach Authorization Middleware Validate

Scope: applies to `apps/workflows/src/checker/**`, `apps/workflows/src/cron/**`, `apps/workflows/src/incident/**`.

These rules are ALWAYS ACTIVE for all HTTP-triggered workflow modules, background cron dispatchers, and monitor checker services.

### Rules

- **R-WF-001** MUST: Route definitions MUST attach authorization middleware to validate request credentials prior to executing downstream business logic or workflow triggers.

### Verify

```bash
# Discover the project testing script from the dependency manifest and execute the test suite covering workflow routing endpoints.
# Discover the project static analysis and lint script from the repository manifest and run validation against all route definition modules.
# Discover the type validation command defined in the project configuration and verify type compatibility across all route handlers.
```

**Accept when:**
- All workflow endpoints are registered on framework router instances and successfully pass automated HTTP integration tests.
- Requests omitting valid authorization headers are rejected with an unauthorized status prior to downstream handler execution.
- Type validation and lint verification scripts execute without errors across all router and handler modules.

<enforcement>
Claude Code MUST NOT skip or defer verification. All workflow services and cron tasks must enforce authorization middleware via the framework routing layer.
</enforcement>