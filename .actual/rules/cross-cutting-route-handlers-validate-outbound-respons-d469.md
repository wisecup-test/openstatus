---
version: 2
name: cross-cutting-route-handlers-validate-outbound-respons-d469
description: "Adoption of @hono/zod-openapi for HTTP Route Definitions and Schema Validation: Route Handlers Validate Outbound Response Payloads"
metadata:
  actual:
    adr_id: d469beb3-d1cd-4aba-9c85-d8dd9fa72380
    status: accepted
    candidate_group_id: 36ec8bb5-66f0-4c12-909f-e2501da343e4
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Adoption of @hono/zod-openapi for HTTP Route Definitions and Schema Validation: Route Handlers Validate Outbound Response Payloads

Scope: applies to `apps/server/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ROUTE-001** MUST: Route handlers MUST validate all outbound response payloads and incoming request parameters using declared schema parse methods prior to returning data to clients.

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