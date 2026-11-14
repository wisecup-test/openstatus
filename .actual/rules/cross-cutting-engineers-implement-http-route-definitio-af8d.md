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

- **R-HONO-001** MUST: Engineers MUST implement HTTP API route definitions and endpoint handlers using @hono/zod-openapi to enforce synchronized runtime validation and OpenAPI contract generation.
- **R-HONO-002** MANDATORY: Prior to writing code that uses a versioned library, execute lock-version grounding: find the dependency manifest, identify the build tool, inspect the repository lock or resolution artifact to determine exact resolved versions, look up official documentation for that exact version, and confirm every API/class/function exists in that version.
- **R-HONO-003** MANDATORY: Define reusable entity schemas centrally and use composition methods to derive collection schemas and partial update schemas across route handlers.
- **R-HONO-004** MANDATORY: Inject request-scoped dependencies through framework context middleware and retrieve them in route handlers via context accessors.

### Verify

```bash
# Discover the repository dependency manifest and lock artifact, and execute project typecheck and test verification scripts
# Discover and run static analysis and linting scripts
# Discover and run the project test execution script
```

**Accept when:**
- All route handlers compile cleanly with complete type inference derived from declared schema definitions.
- API test suites pass with all endpoints rejecting invalid input payloads and returning schema-compliant responses.
- All contextual state access inside route handlers conforms to framework context getter patterns.

<enforcement>
Claude Code MUST NOT skip or defer verification. All new and modified HTTP route handlers must pass CI static analysis, typechecking, and automated contract and schema validation test suites.
</enforcement>