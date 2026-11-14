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

These rules are ALWAYS ACTIVE for all application route definitions, redirect pages, dynamic slug resolvers, API route handlers, and navigation UI components.

### Rules

- **R-NAV-001** SHOULD_NOT: Components SHOULD NOT access raw URL strings directly when native navigation hooks and primitives can supply parsed state.

### Verify

```bash
# Discover and execute workspace static analysis, linting, and type verification scripts from repository configuration
# Run project automated test suite to validate route resolution and redirect behaviors
```

**Accept when:**
- Static analysis and type checking complete without import violations or navigation hook syntax errors.
- All redirect routes, dynamic slug pages, and API callbacks return expected navigation actions in automated test runs.
- No legacy routing packages are referenced in active route and component files.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>