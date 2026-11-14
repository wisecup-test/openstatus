---
version: 2
name: cross-cutting-application-code-not-import-legacy-c410
description: "Standardization on next/navigation for Application Routing and Navigation Primitives: Application Code Not Import Legacy Router"
metadata:
  actual:
    adr_id: c4103da5-82e2-4f2e-b36a-2b50f0390aa5
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

# Standardization on next/navigation for Application Routing and Navigation Primitives: Application Code Not Import Legacy Router

Scope: applies to `apps/web/src/app/**`, `apps/web/src/content/**`.

These rules are ALWAYS ACTIVE for all application route definitions, redirect pages, dynamic slug resolvers, API route handlers, and navigation UI components.

### Rules

- **R-NAV-001** MUST_NOT: Application code MUST NOT import legacy router interfaces or perform imperative window location mutations for internal navigation flows.

### Verify

```bash
# Discover and run the workspace static analysis, linting, and type verification scripts from the repository configuration
# Example discovery:
# npm run lint
# npm run typecheck
# npm test
```

**Accept when:**
- Static analysis and type checking complete without import violations or navigation hook syntax errors.
- All redirect routes, dynamic slug pages, and API callbacks return expected navigation actions in automated test runs.
- No legacy routing packages are referenced in active route and component files.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>