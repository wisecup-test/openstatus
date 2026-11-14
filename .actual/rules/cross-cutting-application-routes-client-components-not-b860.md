---
version: 2
name: cross-cutting-application-routes-client-components-not-b860
description: "Adoption of @openstatus/api Internal Module for Service Contracts and Procedure Routing: Application Routes Client Components Not Declare"
metadata:
  actual:
    adr_id: b8604973-e36e-4c8c-9e4a-bddfadaac91e
    status: accepted
    candidate_group_id: 6a007d6a-909a-425c-8c5a-15faed6eeac3
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/**
      - src/trpc/**
---

# Adoption of @openstatus/api Internal Module for Service Contracts and Procedure Routing: Application Routes Client Components Not Declare

Scope: applies to `apps/web/src/app/api/**`, `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for all application endpoints, route handlers, webhook receivers, client components, server rendering components, and metadata generators invoking or defining remote procedures.

### Rules

- **R-API-001** MUST_NOT: Application routes and client components MUST NOT declare ad-hoc remote procedure contract types or duplicate shared schema definitions outside of the @openstatus/api package.

### Verify

```bash
# Discover and run the project static analysis and type checking script to verify that all procedure calls match module contracts.
# Discover and run the workspace test suite to ensure server route handlers and client callers integrate without contract violations.
```

**Accept when:**
- All workspace type checks pass with zero signature or contract mismatch errors across all consuming modules.
- All route integration tests pass successfully without runtime errors during procedure invocation or error serialization.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration checks executing workspace type verification and contract test suites, as well as architectural pull request reviews.
</enforcement>