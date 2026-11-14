---
version: 2
name: cross-cutting-server-side-page-components-not-8af6
description: "Adoption of nuqs for Type-Safe Server-Side Search Parameter Parsing: Server Side Page Components Not Directly"
metadata:
  actual:
    adr_id: 8af60a5c-509e-45fd-b925-b54ef5c502e5
    status: accepted
    candidate_group_id: d25c74b8-2b62-4f95-8d16-03b44b81995c
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/app/**
---

# Adoption of nuqs for Type-Safe Server-Side Search Parameter Parsing: Server Side Page Components Not Directly

Scope: applies to `apps/dashboard/src/app/**`.

These rules are ALWAYS ACTIVE for all server-rendered application page routes that consume URL query parameters for filtering, sorting, pagination, or state initialization.

### Rules

- **R-NUQS-001** MUST_NOT: Server-side page components MUST NOT directly access untyped raw search parameter dictionary objects without validating them through searchParamsCache.parse.

### Verify

```bash
# Discover and run the project static analysis and linting scripts
# Discover and execute the test runner to validate route page query parameter parsing and fallback behavior
# Discover and trigger the project type checking suite
```

**Accept when:**
- All server-side page routes parsing URL query parameters execute searchParamsCache.parse without type errors.
- Automated static analysis and type checking scripts complete with zero violations across all route modules.
- Unit and integration test suites validating search parameter parsing pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests containing unvalidated direct access to raw search parameter dictionaries will be blocked by continuous integration.
</enforcement>