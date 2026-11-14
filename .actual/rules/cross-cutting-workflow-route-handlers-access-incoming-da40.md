---
version: 2
name: cross-cutting-workflow-route-handlers-access-incoming-da40
description: "Adopt Hono Framework for Workflow Service Routing and Middleware: Workflow Route Handlers Access Incoming Request"
metadata:
  actual:
    adr_id: da4018dc-0347-40fe-b99c-e86926576627
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

# Adopt Hono Framework for Workflow Service Routing and Middleware: Workflow Route Handlers Access Incoming Request

Scope: applies to `apps/workflows/src/checker/**`, `apps/workflows/src/cron/**`, `apps/workflows/src/incident/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-WF-001** MUST: Workflow route handlers MUST access incoming request context parameters, payloads, and environment bindings through the framework context abstraction rather than global runtime state.

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
Claude Code MUST NOT skip or defer verification.
</enforcement>