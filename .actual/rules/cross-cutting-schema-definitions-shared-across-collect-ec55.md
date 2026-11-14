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
- **R-HONO-002** MANDATORY (DISCOVERY): The consumer MUST derive all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-HONO-003** MANDATORY (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, execute in order: find dependency manifest, identify build tool, inspect repository lock or resolution artifact for exact resolved version, look up official documentation for that exact version, confirm every API/class/function exists in that version, and re-run per dependency at point of use for version-sensitive behavior.
- **R-HONO-004** MANDATORY: Define reusable entity schemas centrally and use composition methods to derive collection schemas and partial update schemas across route handlers.
- **R-HONO-005** MANDATORY: Inject request-scoped dependencies through framework context middleware and retrieve them in route handlers via context accessors.

### Verify

```bash
# Discover the repository dependency manifest and lock artifact, and execute the project typecheck and test verification scripts
# Discover and run the project static analysis and linting scripts
# Discover and run the project test execution script
```

**Accept when:**
- All route handlers compile cleanly with complete type inference derived from declared schema definitions.
- API test suites pass with all endpoints rejecting invalid input payloads and returning schema-compliant responses.
- All contextual state access inside route handlers conforms to framework context getter patterns.

<enforcement>
Compliance with these rules is enforced via continuous integration static analysis, peer code reviews, and automated contract/schema validation test suites. Non-compliant implementations are blocked from merging.
</enforcement>