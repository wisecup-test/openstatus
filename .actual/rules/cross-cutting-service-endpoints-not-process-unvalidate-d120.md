---
version: 2
name: cross-cutting-service-endpoints-not-process-unvalidate-d120
description: "@hono/zod-validator Route Boundary Schema Validation: Service Endpoints Not Process Unvalidated Request"
metadata:
  actual:
    adr_id: d120a71c-0e26-40b5-bcbb-c66dd82fe230
    status: accepted
    candidate_group_id: df547031-70ee-405b-a1af-65a995e2e494
    evidence:
      source: deep
  scope:
    root: apps/screenshot-service
    paths:
      - src/**
---

# @hono/zod-validator Route Boundary Schema Validation: Service Endpoints Not Process Unvalidated Request

Scope: applies to `apps/screenshot-service/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HNV-001** MUST_NOT: Service endpoints MUST NOT process unvalidated request bodies or bypass schema parsing middleware at ingress boundaries.

### Verify

```bash
# Discover the project build and type checking configuration to execute static analysis across all service boundary files.
# Discover the project test suite runner and execute automated boundary validation test suites against defined HTTP routes.
```

**Accept when:**
- All service endpoints reject invalid payloads with client error status codes prior to executing downstream tasks.
- Static type analysis verifies zero type mismatches between validated route payloads and downstream business logic handlers.

<enforcement>
Claude Code MUST NOT skip or defer verification. Violation handling: Pull requests introducing unvalidated HTTP ingress routes fail automated verification checks, and code review blocks merge until schema validation middleware is bound to the route definition.
</enforcement>