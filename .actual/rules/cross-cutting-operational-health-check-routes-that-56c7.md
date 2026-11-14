---
version: 2
name: cross-cutting-operational-health-check-routes-that-56c7
description: "@hono/zod-validator Route Boundary Schema Validation: Operational Health Check Routes That Not"
metadata:
  actual:
    adr_id: 56c79499-5283-47d5-bc5d-c07d8d3585d1
    status: accepted
    candidate_group_id: df547031-70ee-405b-a1af-65a995e2e494
    evidence:
      source: deep
  scope:
    root: apps/screenshot-service
    paths:
      - src/**
---

# @hono/zod-validator Route Boundary Schema Validation: Operational Health Check Routes That Not

Scope: applies to `apps/screenshot-service/src/**`.

These rules are ALWAYS ACTIVE for HTTP route handlers defining public or inter-service API boundaries accepting structured input payloads.

### Rules

- **R-ROUTE-001** SHOULD: Operational health check routes that do not accept request payloads SHOULD be defined without payload validation middleware.

### Verify

```bash
# Discover the project build, type checking, and test configuration, then run static analysis and tests.
# For example:
npm run build
npm run test
```

**Accept when:**
- All service endpoints reject invalid payloads with client error status codes prior to executing downstream tasks.
- Static type analysis verifies zero type mismatches between validated route payloads and downstream business logic handlers.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration checks execute static type validation and automated boundary test suites.
</enforcement>