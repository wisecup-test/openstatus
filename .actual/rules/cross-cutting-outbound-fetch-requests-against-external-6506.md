---
version: 2
name: cross-cutting-outbound-fetch-requests-against-external-6506
description: "Fetch API Client Boundary Integration: Outbound Fetch Requests Against External Third"
metadata:
  actual:
    adr_id: 6506d775-0561-4295-ba96-81b47da861db
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

# Fetch API Client Boundary Integration: Outbound Fetch Requests Against External Third

Scope: applies to `apps/web/src/app/**`, `apps/web/src/content/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for all client-side components, hooks, server-side utility functions, background checkers, and route handlers communicating across external HTTP boundaries.

### Rules

- **R-FETCH-001** MUST: Outbound fetch requests against external third-party endpoints or long-running internal operations MUST provide an abort signal to guarantee bounded execution time and prevent unhandled request hangs.

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
Claude Code MUST NOT skip or defer verification.
</enforcement>