---
version: 2
name: cross-cutting-developers-inspect-repository-lock-artif-a35f
description: "@hono/zod-validator Route Boundary Schema Validation: Developers Inspect Repository Lock Artifact Verify"
metadata:
  actual:
    adr_id: a35fcc6d-13cf-4ee5-b1fb-5f5ab0f58b90
    status: accepted
    candidate_group_id: df547031-70ee-405b-a1af-65a995e2e494
    evidence:
      source: deep
  scope:
    root: apps/screenshot-service
    paths:
      - src/**
---

# @hono/zod-validator Route Boundary Schema Validation: Developers Inspect Repository Lock Artifact Verify

Scope: applies to `apps/screenshot-service/src/**`.

These rules are ALWAYS ACTIVE for HTTP route handlers defining public or inter-service API boundaries accepting structured input payloads.

### Rules

- **R-HZN-001** MUST: Developers MUST inspect the repository lock artifact to verify the exact resolved version of @hono/zod-validator and its associated schema dependencies before implementing or modifying route validation bindings.

### Verify

```bash
# Discover the project build and type checking configuration to execute static analysis across all service boundary files.
# Discover the project test suite runner and execute automated boundary validation test suites against defined HTTP routes.
```

**Accept when:**
- All service endpoints reject invalid payloads with client error status codes prior to executing downstream tasks.
- Static type analysis verifies zero type mismatches between validated route payloads and downstream business logic handlers.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>