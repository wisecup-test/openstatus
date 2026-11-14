---
version: 2
name: cross-cutting-proxy-utility-functions-derive-their-f4b6
description: "Centralized Route Resolution via resolve-route Module: Proxy Utility Functions Derive Their Path"
metadata:
  actual:
    adr_id: f4b602b6-0dc0-41f3-af90-012ca4d8a44d
    status: accepted
    candidate_group_id: c1192f46-e2dc-4500-93bd-53907a2f23e6
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/lib/proxy/**
---

# Centralized Route Resolution via resolve-route Module: Proxy Utility Functions Derive Their Path

Scope: applies to `apps/status-page/src/lib/proxy/**`.

These rules are ALWAYS ACTIVE for proxy request handling, URL construction, locale override resolution, and path prefix normalization routines.

### Rules

- **R-PROX-001** SHOULD: Proxy utility functions derive their path structures and parameter types directly from contracts exported by the resolve-route module.

### Verify

```bash
# Discover and execute the workspace static analysis, linting, type checking, and test scripts
# to verify proxy path manipulation utilities import and utilize resolve-route.
# (Exact commands depend on the repository manifest and build configuration)
```

**Accept when:**
- All proxy path transformation functions import and route path parsing through the resolve-route module.
- Repository test suites and type verification scripts pass without route resolution errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. All changes affecting proxy path manipulation or the resolve-route module must comply with centralized route resolution and pass continuous integration checks and mandatory peer code review.
</enforcement>