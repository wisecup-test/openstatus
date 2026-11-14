---
version: 2
name: cross-cutting-developers-discover-authoritative-reposi-9bc9
description: "Standardization on next/navigation for Application Routing and Navigation Primitives: Developers Discover Authoritative Repository Lock Artifact"
metadata:
  actual:
    adr_id: 9bc9c7f6-6927-477e-8b74-9b8848c20b47
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

# Standardization on next/navigation for Application Routing and Navigation Primitives: Developers Discover Authoritative Repository Lock Artifact

Scope: applies to `apps/web/src/app/**`, `apps/web/src/content/**`.

These rules are ALWAYS ACTIVE for all application route definitions, redirect pages, dynamic slug resolvers, API route handlers, and navigation UI components.

### Rules

- **R-NAV-001** MUST: Developers MUST discover the authoritative repository lock artifact and verify the exact resolved version of the navigation library against upstream documentation before introducing or updating navigation APIs.

### Verify

```bash
# Discover and execute workspace static analysis and linting scripts
# Discover and run project type verification script
# Discover and run the project automated test suite
```

**Accept when:**
- Static analysis and type checking complete without import violations or navigation hook syntax errors.
- All redirect routes, dynamic slug pages, and API callbacks return expected navigation actions in automated test runs.
- No legacy routing packages are referenced in active route and component files.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification via automated test suites, linting, and type checking is mandatory for all navigation and routing changes.
</enforcement>