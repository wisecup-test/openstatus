---
version: 2
name: cross-cutting-server-route-components-accepting-url-f35a
description: "Adoption of nuqs for Server-Side URL Search Parameter Parsing and Validation: Server Route Components Accepting Url Search"
metadata:
  actual:
    adr_id: f35ab5ee-9f05-4490-bcef-5eb69d96fc1f
    status: accepted
    candidate_group_id: b6caa4e1-a734-4f95-91e7-b9071cb6b4c1
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Adoption of nuqs for Server-Side URL Search Parameter Parsing and Validation: Server Route Components Accepting Url Search

Scope: applies to `apps/status-page/src/app/**`.

These rules are ALWAYS ACTIVE for server-rendered route page components that receive and process URL search parameters and shared search parameter definitions consumed by both server page handlers and client components.

### Rules

- **R-NUQS-001** MUST: Server route components accepting URL search parameters MUST parse and validate incoming query inputs using dedicated search parameter cache parsers provided by nuqs/server prior to consuming parameter values.

### Verify

```bash
# Discover and execute the repository static type checker to confirm that all search parameter parsing calls adhere to defined schema types.
# Discover and run the project test suite to verify route parameter parsing and validation error handling.
```

**Accept when:**
- Static type checking and automated test suites confirm all route search parameters pass through validated cache parser instances.
- No server route handlers access raw search query parameters directly without parsing.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated static analysis, linting rules, and peer code review.
</enforcement>