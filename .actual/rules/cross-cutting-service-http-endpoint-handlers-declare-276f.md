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
# Discover and run the project build, type checking, and test suite runner to verify boundary validation configuration.
npm run build || pnpm build || yarn build
npm run test || pnpm test || yarn test
```

**Accept when:**
- All service endpoints reject invalid payloads with client error status codes prior to executing downstream tasks.
- Static type analysis verifies zero type mismatches between validated route payloads and downstream business logic handlers.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration checks, automated boundary test suites, and peer reviews enforce these rules.
</enforcement>