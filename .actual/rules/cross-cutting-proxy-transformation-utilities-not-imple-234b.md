---
version: 2
name: cross-cutting-proxy-transformation-utilities-not-imple-234b
description: "Centralized Route Resolution via resolve-route Module: Proxy Transformation Utilities Not Implement Bespoke"
metadata:
  actual:
    adr_id: 234bd199-22cc-4a79-9271-112bc86b1be3
    status: accepted
    candidate_group_id: c1192f46-e2dc-4500-93bd-53907a2f23e6
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/lib/proxy/**
---

# Centralized Route Resolution via resolve-route Module: Proxy Transformation Utilities Not Implement Bespoke

Scope: applies to `apps/status-page/src/lib/proxy/**`.

These rules are ALWAYS ACTIVE for proxy request handling, URL construction, locale override resolution, and path prefix normalization routines.

### Rules

- **R-PROXY-001** MUST_NOT: Proxy transformation utilities MUST NOT implement bespoke path parsing, ad-hoc string splitting, or unvetted regular expressions for route resolution.

### Verify

```bash
# Discover and run the workspace static analysis, linting, type checking, and test scripts from the dependency manifest
# Example discovery workflow:
# 1. Inspect package.json / dependency manifest for lint/test scripts
# 2. Run the discovered scripts to verify all proxy path manipulation utilities import and utilize resolve-route
npm run lint
npm run typecheck
npm test
```

**Accept when:**
- All proxy path transformation functions import and route path parsing through the resolve-route module.
- Repository test suites and type verification scripts pass without route resolution errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration checks executing repository test suites and static analysis, and mandatory peer code review.
</enforcement>