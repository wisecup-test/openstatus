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

These rules are ALWAYS ACTIVE for HTTP route handler modules and endpoint definitions within the API server, as well as request and response validation pipelines for REST API services.

### Rules

- **R-HONO-001** MUST_NOT: Route handlers MUST_NOT return unvalidated database entities or raw internal data representations directly without passing through schema validation.

### Verify

```bash
# Discover the repository dependency manifest and lock artifact, and execute typecheck and test verification scripts
# Example discovery and execution pattern:
# 1. Identify package manager and run typecheck
# 2. Run static analysis and linting scripts
# 3. Run test execution suite
```

**Accept when:**
- All route handlers compile cleanly with complete type inference derived from declared schema definitions.
- API test suites pass with all endpoints rejecting invalid input payloads and returning schema-compliant responses.
- All contextual state access inside route handlers conforms to framework context getter patterns.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests violating schema validation or route conventions are blocked by continuous integration.
</enforcement>