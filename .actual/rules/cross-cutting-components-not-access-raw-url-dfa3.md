---
version: 2
name: cross-cutting-components-not-access-raw-url-dfa3
description: "Standardization on next/navigation for Application Routing and Navigation Primitives: Components Not Access Raw Url Strings"
metadata:
  actual:
    adr_id: dfa3298c-9e72-4661-9287-bc58c8cbf594
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

# Standardization on next/navigation for Application Routing and Navigation Primitives: Components Not Access Raw Url Strings

Scope: applies to `apps/web/src/app/**`, `apps/web/src/content/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NAV-001** SHOULD_NOT: Components SHOULD NOT access raw URL strings directly when native navigation hooks and primitives can supply parsed state.

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