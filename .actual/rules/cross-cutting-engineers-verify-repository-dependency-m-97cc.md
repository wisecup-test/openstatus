---
version: 2
name: cross-cutting-engineers-verify-repository-dependency-m-97cc
description: "Adoption of @hono/zod-openapi for HTTP Route Definitions and Schema Validation: Engineers Verify Repository Dependency Manifest Lock"
metadata:
  actual:
    adr_id: 97ccc246-8c30-4819-9764-596f3e5a188f
    status: accepted
    candidate_group_id: 36ec8bb5-66f0-4c12-909f-e2501da343e4
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Adoption of @hono/zod-openapi for HTTP Route Definitions and Schema Validation: Engineers Verify Repository Dependency Manifest Lock

Scope: applies to `apps/server/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HZN-001** MUST: Engineers MUST verify the repository dependency manifest and lock artifact to resolve the exact locked version of @hono/zod-openapi and validate API compatibility against official documentation before implementation.

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
Claude Code MUST NOT skip or defer verification.
</enforcement>