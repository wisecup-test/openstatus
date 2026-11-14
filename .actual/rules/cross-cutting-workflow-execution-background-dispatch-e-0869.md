---
version: 2
name: cross-cutting-workflow-execution-background-dispatch-e-0869
description: "Adopt Hono Framework for Workflow Service Routing and Middleware: Workflow Execution Background Dispatch Endpoints Constructed"
metadata:
  actual:
    adr_id: 08694dd3-12f0-4001-8090-fd6cbf1c61ea
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

# Adopt Hono Framework for Workflow Service Routing and Middleware: Workflow Execution Background Dispatch Endpoints Constructed

Scope: applies to `apps/workflows/src/checker/**`, `apps/workflows/src/cron/**`, `apps/workflows/src/incident/**`.

These rules are ALWAYS ACTIVE for all HTTP-triggered workflow modules, background cron dispatchers, monitor checker services, and all associated middleware layers enforcing authentication, authorization, or context binding.

### Rules

- **R-HONO-001** MUST: Workflow execution and background dispatch endpoints MUST be constructed using the Hono framework application router to define HTTP interfaces and task dispatch boundaries.

### Verify

```bash
# Discover and execute the project testing script covering workflow routing endpoints:
# (Example: npm test / yarn test / pnpm test)

# Discover and execute the project static analysis and lint script:
# (Example: npm run lint / yarn lint / pnpm lint)

# Discover and execute the type validation command:
# (Example: npm run typecheck / tsc --noEmit)
```

**Accept when:**
- All workflow endpoints are registered on framework router instances and successfully pass automated HTTP integration tests.
- Requests omitting valid authorization headers are rejected with an unauthorized status prior to downstream handler execution.
- Type validation and lint verification scripts execute without errors across all router and handler modules.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration test suites and peer code reviews. Pull requests containing unrouted handlers or missing authorization middleware are blocked from merging.
</enforcement>