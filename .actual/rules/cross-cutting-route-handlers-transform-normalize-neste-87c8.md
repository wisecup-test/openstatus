---
version: 2
name: cross-cutting-route-handlers-transform-normalize-neste-87c8
description: "Adoption of @hono/zod-openapi for HTTP Route Definitions and Schema Validation: Route Handlers Transform Normalize Nested Serialized"
metadata:
  actual:
    adr_id: 87c8fa77-f274-45e8-a78e-67ee16860b2d
    status: accepted
    candidate_group_id: 36ec8bb5-66f0-4c12-909f-e2501da343e4
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Adoption of @hono/zod-openapi for HTTP Route Definitions and Schema Validation: Route Handlers Transform Normalize Nested Serialized

Scope: applies to `apps/server/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MAY: Route handlers MAY transform and normalize nested serialized structures within schema parsing pipelines to align data payloads with endpoint contracts.

### Verify

```bash
# Discover dependency manifest/lock and execute typecheck and test verification scripts
# Discover and run static analysis and linting scripts
# Discover and run test execution script
```

**Accept when:**
- All route handlers compile cleanly with complete type inference derived from declared schema definitions.
- API test suites pass with all endpoints rejecting invalid input payloads and returning schema-compliant responses.
- All contextual state access inside route handlers conforms to framework context getter patterns.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>