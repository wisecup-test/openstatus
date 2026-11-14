---
version: 2
name: cross-cutting-schema-definitions-shared-across-collect-ec55
description: "Adoption of @hono/zod-openapi for HTTP Route Definitions and Schema Validation: Schema Definitions Shared Across Collection Individual"
metadata:
  actual:
    adr_id: ec55a52c-99b5-4d18-ac43-671967523d71
    status: accepted
    candidate_group_id: 36ec8bb5-66f0-4c12-909f-e2501da343e4
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Adoption of @hono/zod-openapi for HTTP Route Definitions and Schema Validation: Schema Definitions Shared Across Collection Individual

Scope: applies to `apps/server/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HONO-001** SHOULD: Schema definitions SHOULD be shared across collection and individual entity route handlers using schema array transformations rather than duplicated inline definitions.
- **R-HONO-002** MANDATORY: Define reusable entity schemas centrally and use composition methods to derive collection schemas and partial update schemas across route handlers.
- **R-HONO-003** MANDATORY: Inject request-scoped dependencies through framework context middleware and retrieve them in route handlers via context accessors.
- **R-HONO-004** MANDATORY (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, execute in order: (1) Find dependency manifest in the repo; (2) Identify the build tool; (3) Inspect repository lock/resolution artifact for exact resolved version; (4) Look up official documentation for that exact version; (5) Confirm every API/class/function called exists in that version; (6) Re-run steps 3-5 per dependency at point of use for version-sensitive behavior.

### Verify

```bash
# Discover the repository dependency manifest and lock artifact, and execute typecheck and test verification scripts
# Discover and run static analysis and linting scripts
# Discover and run test execution scripts
```

**Accept when:**
- All route handlers compile cleanly with complete type inference derived from declared schema definitions.
- API test suites pass with all endpoints rejecting invalid input payloads and returning schema-compliant responses.
- All contextual state access inside route handlers conforms to framework context getter patterns.

<enforcement>
Claude Code MUST NOT skip or defer verification. Violation handling: Pull requests violating schema validation or route conventions are blocked by continuous integration, and non-compliant route implementations must be refactored before merging.
</enforcement>