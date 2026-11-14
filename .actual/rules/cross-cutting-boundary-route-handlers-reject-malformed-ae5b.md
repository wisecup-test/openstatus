---
version: 2
name: cross-cutting-boundary-route-handlers-reject-malformed-ae5b
description: "@hono/zod-validator Route Boundary Schema Validation: Boundary Route Handlers Reject Malformed Payload"
metadata:
  actual:
    adr_id: ae5b4b0b-b633-4948-a023-efbdb7d5e437
    status: accepted
    candidate_group_id: df547031-70ee-405b-a1af-65a995e2e494
    evidence:
      source: deep
  scope:
    root: apps/screenshot-service
    paths:
      - src/**
---

# @hono/zod-validator Route Boundary Schema Validation: Boundary Route Handlers Reject Malformed Payload

Scope: applies to `apps/screenshot-service/src/**`.

These rules are ALWAYS ACTIVE for all HTTP route handlers defining public or inter-service API boundaries accepting structured input payloads.

### Rules

- **R-ZOD-001** MUST: Boundary route handlers MUST reject malformed payload data before triggering downstream asynchronous tasks, storage actions, or persistence mutations.

### Verify

```bash
# Discover the project build and type checking configuration to execute static analysis across all service boundary files.
# Discover the project test suite runner and execute automated boundary validation test suites against defined HTTP routes.
```

**Accept when:**
- All service endpoints reject invalid payloads with client error status codes prior to executing downstream tasks.
- Static type analysis verifies zero type mismatches between validated route payloads and downstream business logic handlers.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration checks, automated boundary validation test suites, and peer review verify compliance; pull requests introducing unvalidated HTTP ingress routes fail automated verification checks.
</enforcement>