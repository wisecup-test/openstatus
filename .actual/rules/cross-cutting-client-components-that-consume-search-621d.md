---
version: 2
name: cross-cutting-client-components-that-consume-search-621d
description: "Standardization on next/navigation for Application Routing and Navigation Primitives: Client Components That Consume Search Parameters"
metadata:
  actual:
    adr_id: 621d1143-185c-4436-bea3-db98e1ce3a00
    status: accepted
    candidate_group_id: b5794d4a-1fde-45ec-8132-55f07184628d
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
      - src/content/**
---

# Standardization on next/navigation for Application Routing and Navigation Primitives: Client Components That Consume Search Parameters

Scope: applies to `apps/web/src/app/**`, `apps/web/src/content/**`.

These rules are ALWAYS ACTIVE for all application route definitions, redirect pages, dynamic slug resolvers, API route handlers, and navigation UI components.

### Rules

- **R-NAV-001** MUST: Client components that consume search parameters, route paths, or perform client-side transitions MUST use the corresponding client hooks from next/navigation and declare the appropriate client execution boundary directive.

### Verify

```bash
# Discover and run the workspace static analysis and linting scripts
# Discover and run the project type verification script
# Discover and run the project automated test suite
```

**Accept when:**
- Static analysis and type checking complete without import violations or navigation hook syntax errors.
- All redirect routes, dynamic slug pages, and API callbacks return expected navigation actions in automated test runs.
- No legacy routing packages are referenced in active route and component files.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>