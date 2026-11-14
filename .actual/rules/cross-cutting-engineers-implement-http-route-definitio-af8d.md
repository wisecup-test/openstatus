---
version: 2
name: cross-cutting-engineers-implement-http-route-definitio-af8d
description: "Adoption of @hono/zod-openapi for HTTP Route Definitions and Schema Validation: Engineers Implement Http Route Definitions Endpoint"
metadata:
  actual:
    adr_id: af8d3d87-eb3b-44e1-bc6f-5c1769890b8f
    status: accepted
    candidate_group_id: 36ec8bb5-66f0-4c12-909f-e2501da343e4
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Adoption of @hono/zod-openapi for HTTP Route Definitions and Schema Validation: Engineers Implement Http Route Definitions Endpoint

Scope: applies to `apps/server/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HONO-001** MUST: Implement HTTP API route definitions and endpoint handlers using @hono/zod-openapi to enforce synchronized runtime validation and OpenAPI contract generation.

### Verify

```bash
# Discover and run the project typecheck, test, lint, and static analysis verification scripts as defined in the repository manifest.
```

**Accept when:**
- All route handlers compile cleanly with complete type inference derived from declared schema definitions.
- API test suites pass with all endpoints rejecting invalid input payloads and returning schema-compliant responses.
- All contextual state access inside route handlers conforms to framework context getter patterns.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>