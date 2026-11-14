---
version: 2
name: cross-cutting-service-http-endpoint-handlers-declare-276f
description: "@hono/zod-validator Route Boundary Schema Validation: Service Http Endpoint Handlers Declare Incoming"
metadata:
  actual:
    adr_id: 276fca9c-3dce-44b1-a21b-48157adafbf4
    status: accepted
    candidate_group_id: df547031-70ee-405b-a1af-65a995e2e494
    evidence:
      source: deep
  scope:
    root: apps/screenshot-service
    paths:
      - src/**
---

# @hono/zod-validator Route Boundary Schema Validation: Service Http Endpoint Handlers Declare Incoming

Scope: applies to `apps/screenshot-service/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HNV-001** MUST: Service HTTP endpoint handlers MUST declare incoming request payload constraints using @hono/zod-validator middleware at the service boundary definition prior to executing business logic.

### Verify

```bash
# Discover and execute project build/type checking and test suite runner to verify boundary validation and type safety.
# (Project-specific commands to be derived from repository configuration)
```

**Accept when:**
- All service endpoints reject invalid payloads with client error status codes prior to executing downstream tasks.
- Static type analysis verifies zero type mismatches between validated route payloads and downstream business logic handlers.

<enforcement>
Claude Code MUST NOT skip or defer verification. Violation handling: Pull requests introducing unvalidated HTTP ingress routes fail automated verification checks, and code review blocks merge until schema validation middleware is bound to the route definition.
</enforcement>