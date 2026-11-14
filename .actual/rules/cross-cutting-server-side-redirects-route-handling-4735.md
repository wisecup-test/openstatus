---
version: 2
name: cross-cutting-server-side-redirects-route-handling-4735
description: "Standardization on next/navigation for Application Routing and Navigation Primitives: Server Side Redirects Route Handling Aborts"
metadata:
  actual:
    adr_id: 4735eff7-f1cd-43b1-b446-39d121519d47
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

# Standardization on next/navigation for Application Routing and Navigation Primitives: Server Side Redirects Route Handling Aborts

Scope: applies to `apps/web/src/app/**`, `apps/web/src/content/**`.

These rules are ALWAYS ACTIVE for all application route definitions, redirect pages, dynamic slug resolvers, API route handlers, and navigation UI components.

### Rules

- **R-NAV-001** SHOULD: Server-side redirects and route handling aborts execute via the server-compatible control flow functions exposed by next/navigation.

### Verify

```bash
# Discover the workspace static analysis and linting scripts from the repository configuration and execute them across the application source tree.
# Discover the project type verification script and run it to ensure navigation hook and parameter types adhere to upstream contracts.
# Discover and run the project automated test suite to validate route resolution and redirect behaviors across all endpoints.
```

**Accept when:**
- Static analysis and type checking complete without import violations or navigation hook syntax errors.
- All redirect routes, dynamic slug pages, and API callbacks return expected navigation actions in automated test runs.
- No legacy routing packages are referenced in active route and component files.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests containing disallowed router imports or direct window location navigation will fail automated validation checks.
</enforcement>