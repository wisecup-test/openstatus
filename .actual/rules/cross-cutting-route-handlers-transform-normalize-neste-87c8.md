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

These rules are ALWAYS ACTIVE for all HTTP route handler modules and endpoint definitions within the API server, as well as request and response validation pipelines for REST API services.

### Rules

- **R-HONO-001** MAY: Route handlers MAY transform and normalize nested serialized structures within schema parsing pipelines to align data payloads with endpoint contracts.

### Verify

```bash
# Discover the repository dependency manifest and lock artifact, and execute typecheck and test verification scripts
# Example discovery and execution commands (adapt to project manifest):
if [ -f "package.json" ]; then
  npm test
  npm run typecheck
  npm run lint
fi
```

**Accept when:**
- All route handlers compile cleanly with complete type inference derived from declared schema definitions.
- API test suites pass with all endpoints rejecting invalid input payloads and returning schema-compliant responses.
- All contextual state access inside route handlers conforms to framework context getter patterns.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipelines, static analysis, and automated contract and schema validation test suites strictly enforce these rules, blocking non-compliant pull requests.
</enforcement>