---
version: 2
name: cross-cutting-application-routes-handlers-user-interfa-f234
description: "Standardization on next/navigation for Application Routing and Navigation Primitives: Application Routes Handlers User Interface Components"
metadata:
  actual:
    adr_id: f23496b5-9077-4948-b6f2-9f0507f27718
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

# Standardization on next/navigation for Application Routing and Navigation Primitives: Application Routes Handlers User Interface Components

Scope: applies to `apps/web/src/app/**`, `apps/web/src/content/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NAV-001** MUST: All application routes, handlers, and user interface components requiring navigation control, programmatic redirection, or URL state inspection MUST use the navigation primitives provided by next/navigation.

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
Claude Code MUST NOT skip or defer verification.
</enforcement>