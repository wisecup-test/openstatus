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

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NAV-001** SHOULD: Server-side redirects and route handling aborts SHOULD execute via the server-compatible control flow functions exposed by next/navigation.

### Verify

```bash
# Discover and run workspace static analysis and linting scripts
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