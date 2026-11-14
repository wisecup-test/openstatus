---
version: 2
name: cross-cutting-route-components-import-server-specific-e03d
description: "Adoption of nuqs for Type-Safe Server-Side Search Parameter Parsing: Route Components Import Server Specific Utilities"
metadata:
  actual:
    adr_id: e03d97c4-71da-4b43-b64b-d6d14d1f8de4
    status: accepted
    candidate_group_id: d25c74b8-2b62-4f95-8d16-03b44b81995c
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/app/**
---

# Adoption of nuqs for Type-Safe Server-Side Search Parameter Parsing: Route Components Import Server Specific Utilities

Scope: applies to `apps/dashboard/src/app/**`.

These rules are ALWAYS ACTIVE for all server-rendered application page routes that consume URL query parameters for filtering, sorting, pagination, or state initialization.

### Rules

- **R-NUQS-001** SHOULD: Route components SHOULD import server-specific utilities from the designated server subpath of the query parameter library when executing in server execution contexts.
- **R-NUQS-002** MANDATORY: Declare search parameter parsers using the library schema definitions in dedicated configuration modules co-located with the respective route.
- **R-NUQS-003** MANDATORY: Execute searchParamsCache.parse at the root of the server page component before passing state to downstream data fetchers or client rendering components.

### Verify

```bash
# Discover and run the project static analysis and linting scripts to verify adherence to search parameter import and parsing rules.
# Discover and execute the test runner to validate route page query parameter parsing and fallback behavior.
# Discover and trigger the project type checking suite to ensure all search parameter types conform to declared schemas.
```

**Accept when:**
- All server-side page routes parsing URL query parameters execute searchParamsCache.parse without type errors.
- Automated static analysis and type checking scripts complete with zero violations across all route modules.
- Unit and integration test suites validating search parameter parsing pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>