---
version: 2
name: cross-cutting-route-handlers-generating-dynamic-endpoi-80b2
description: "Adoption of @openstatus/db Module for Shared Schema and Persistence: Route Handlers Generating Dynamic Endpoints Metadata"
metadata:
  actual:
    adr_id: 80b2216f-0f62-4044-86da-5ed973adc9b3
    status: accepted
    candidate_group_id: 5edccb20-8590-46b8-98ed-d0c4028cd2d5
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Adoption of @openstatus/db Module for Shared Schema and Persistence: Route Handlers Generating Dynamic Endpoints Metadata

Scope: applies to `apps/status-page/src/app/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DB-001** SHOULD: Route handlers generating dynamic endpoints or metadata SHOULD resolve incoming request boundaries via runtime header utilities before querying @openstatus/db with tenant-scoped parameters.
- **R-DB-002** MANDATORY: Consumers MUST import database client instances and schema structures through the primary export entry points provided by @openstatus/db rather than deep-linking into unexported internal files.
- **R-DB-003** MANDATORY (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, execute in order: find dependency manifest, identify build tool, inspect lock/resolution artifact, look up official documentation for exact version, confirm every API exists in that exact version's documentation, re-run per dependency at point of use.
- **R-DB-004** MANDATORY: When constructing query filters inside server route handlers, extract request parameters from request headers and pass them as sanitized inputs into queries constructed through the shared database module.

### Verify

```bash
# Discover workspace dependency manifest and execute type-checking and linting suites
# (Commands derived from the repository manifest)
npm run check
npm run lint

# Run the automated test suite for applications consuming @openstatus/db
npm test
```

**Accept when:**
- All imports of @openstatus/db and @openstatus/db/src/schema compile without unresolved module or type errors across the workspace.
- No route or application file declares duplicate local database entity schemas or independent raw connection instances.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by continuous integration checks, peer code reviews, and automated workspace-wide type verification and import boundary linting.
</enforcement>