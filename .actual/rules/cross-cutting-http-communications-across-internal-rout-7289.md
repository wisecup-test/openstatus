---
version: 2
name: cross-cutting-http-communications-across-internal-rout-7289
description: "Fetch API Client Boundary Integration: Http Communications Across Internal Route Boundaries"
metadata:
  actual:
    adr_id: 7289d416-5169-48d5-a4c3-03ba9260aa46
    status: accepted
    candidate_group_id: 57dd9a6d-921b-457c-a0a1-87439126474c
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
      - src/content/**
      - src/lib/**
---

# Fetch API Client Boundary Integration: Http Communications Across Internal Route Boundaries

Scope: applies to `apps/web/src/app/**`, `apps/web/src/content/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for all client-side components, hooks, server-side utility functions, background checkers, and route handlers communicating across internal route boundaries and external service clients.

### Rules

- **R-HTTP-001** MUST: All HTTP communications across internal route boundaries and external service clients MUST use the standard Fetch API rather than third-party HTTP client libraries.

### Verify

```bash
# Discover and run the project static analysis and linting command from the root configuration.
# Discover and execute the project test runner across all integration test suites.
# Discover and execute the project type checking script from the workspace manifest.
```

**Accept when:**
- Static analysis validates that all boundary network requests conform to the standard client rules without unapproved HTTP dependencies.
- All integration test suites covering internal routes and external boundary clients pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification by automated continuous integration pipeline running static analysis, boundary linter rules, and pull request architectural code reviews is mandatory.
</enforcement>