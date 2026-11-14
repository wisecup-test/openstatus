---
version: 2
name: cross-cutting-before-implementing-updating-query-param-4360
description: "Adoption of nuqs for Type-Safe Server-Side Search Parameter Parsing: Before Implementing Updating Query Parameter Parsing"
metadata:
  actual:
    adr_id: 4360074c-b5ca-4108-a0f5-8df3d28672d9
    status: accepted
    candidate_group_id: d25c74b8-2b62-4f95-8d16-03b44b81995c
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/app/**
---

# Adoption of nuqs for Type-Safe Server-Side Search Parameter Parsing: Before Implementing Updating Query Parameter Parsing

Scope: applies to `apps/dashboard/src/app/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NUQS-001** MUST: Before implementing or updating query parameter parsing logic, the consumer MUST inspect the project dependency resolution artifact to determine the exact locked version of the query state library and adhere to its published API specification.

### Verify

```bash
# Discover and run static analysis and linting scripts
# Discover and execute test runner to validate route page query parameter parsing
# Discover and trigger type checking suite
```

**Accept when:**
- All server-side page routes parsing URL query parameters execute searchParamsCache.parse without type errors.
- Automated static analysis and type checking scripts complete with zero violations across all route modules.
- Unit and integration test suites validating search parameter parsing pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>