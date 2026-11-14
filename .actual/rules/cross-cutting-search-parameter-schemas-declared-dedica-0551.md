---
version: 2
name: cross-cutting-search-parameter-schemas-declared-dedica-0551
description: "Adoption of nuqs for Type-Safe Server-Side Search Parameter Parsing: Search Parameter Schemas Declared Dedicated Definition"
metadata:
  actual:
    adr_id: 0551be00-5c84-44b6-9af6-385520c0689f
    status: accepted
    candidate_group_id: d25c74b8-2b62-4f95-8d16-03b44b81995c
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/app/**
---

# Adoption of nuqs for Type-Safe Server-Side Search Parameter Parsing: Search Parameter Schemas Declared Dedicated Definition

Scope: applies to `apps/dashboard/src/app/**`.

These rules are ALWAYS ACTIVE for server-rendered application page routes that consume URL query parameters for filtering, sorting, pagination, or state initialization.

### Rules

- **R-NUQS-001** MUST: Search parameter schemas MUST be declared in dedicated parameter definition modules and imported into both server page entry points and corresponding client consumer components.

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