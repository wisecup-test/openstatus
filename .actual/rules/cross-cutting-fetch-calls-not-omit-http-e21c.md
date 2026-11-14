---
version: 2
name: cross-cutting-fetch-calls-not-omit-http-e21c
description: "Fetch API Client Boundary Integration: Fetch Calls Not Omit Http Error"
metadata:
  actual:
    adr_id: e21ced89-7de6-49f6-b4df-c8e122522377
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

# Fetch API Client Boundary Integration: Fetch Calls Not Omit Http Error

Scope: applies to `apps/web/src/app/**`, `apps/web/src/content/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for all client-side components, hooks, server-side utility functions, background checkers, and route handlers communicating across internal or external HTTP boundaries.

### Rules

- **R-FETCH-001** MUST_NOT: Fetch calls MUST NOT omit HTTP error status handling or assume successful status codes when reading response streams or parsing JSON payloads.

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