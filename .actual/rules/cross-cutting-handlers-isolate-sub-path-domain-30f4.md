---
version: 2
name: cross-cutting-handlers-isolate-sub-path-domain-30f4
description: "Adopt Hono Framework for Workflow Service Routing and Middleware: Handlers Isolate Sub Path Domain Logic"
metadata:
  actual:
    adr_id: 30f448b0-16d1-43ba-8641-745b60a942df
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

# Adopt Hono Framework for Workflow Service Routing and Middleware: Handlers Isolate Sub Path Domain Logic

Scope: applies to `apps/workflows/src/checker/**`, `apps/workflows/src/cron/**`, `apps/workflows/src/incident/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HONO-001** SHOULD: Handlers SHOULD isolate sub-path domain logic into dedicated router instances that are mounted onto the primary application router.

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