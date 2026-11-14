---
version: 2
name: cross-cutting-page-entry-points-pass-parsed-4b0d
description: "Adoption of nuqs for Type-Safe Server-Side Search Parameter Parsing: Page Entry Points Pass Parsed Search"
metadata:
  actual:
    adr_id: 4b0d2b27-2055-42f6-adbc-0795c3c996c6
    status: accepted
    candidate_group_id: d25c74b8-2b62-4f95-8d16-03b44b81995c
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/app/**
---

# Adoption of nuqs for Type-Safe Server-Side Search Parameter Parsing: Page Entry Points Pass Parsed Search

Scope: applies to `apps/dashboard/src/app/**`.

These rules are ALWAYS ACTIVE for server-rendered application page routes that consume URL query parameters for filtering, sorting, pagination, or state initialization.

### Rules

- **R-NUQS-001** MAY: Page entry points MAY pass the parsed search parameter result object directly to downstream data-fetching operations or client view components.

### Verify

```bash
# Discover and run the project static analysis and linting scripts
# Discover and execute the test runner
# Discover and trigger the project type checking suite
```

**Accept when:**
- All server-side page routes parsing URL query parameters execute searchParamsCache.parse without type errors.
- Automated static analysis and type checking scripts complete with zero violations across all route modules.
- Unit and integration test suites validating search parameter parsing pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>