---
version: 2
name: cross-cutting-route-handlers-not-return-unvalidated-1b69
description: "Adoption of @hono/zod-openapi for HTTP Route Definitions and Schema Validation: Route Handlers Not Return Unvalidated Database"
metadata:
  actual:
    adr_id: 1b69add6-2d83-4266-b85c-5c3492ae1cd6
    status: accepted
    candidate_group_id: 36ec8bb5-66f0-4c12-909f-e2501da343e4
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Adoption of @hono/zod-openapi for HTTP Route Definitions and Schema Validation: Route Handlers Not Return Unvalidated Database

Scope: applies to `apps/server/src/**`.

These rules are ALWAYS ACTIVE for HTTP route handler modules, endpoint definitions, and request/response validation pipelines within the API server.

### Rules

- **R-HONO-001** MUST_NOT: Route handlers MUST_NOT return unvalidated database entities or raw internal data representations directly without passing through schema validation.

### Verify

```bash
# Discover the repository dependency manifest and lock artifact, and execute the project typecheck and test verification scripts to confirm route schemas compile without errors.
# Discover and run the project static analysis and linting scripts to verify compliance with route definition and schema parsing standards.
# Discover and run the project test execution script to verify that route handlers validate request parameters and response bodies according to defined contracts.
```

**Accept when:**
- All route handlers compile cleanly with complete type inference derived from declared schema definitions.
- API test suites pass with all endpoints rejecting invalid input payloads and returning schema-compliant responses.
- All contextual state access inside route handlers conforms to framework context getter patterns.

<enforcement>
Claude Code MUST NOT skip or defer verification. All route handlers must use standard route schemas and context accessors, and pull requests violating schema validation or route conventions are blocked by continuous integration.
</enforcement>