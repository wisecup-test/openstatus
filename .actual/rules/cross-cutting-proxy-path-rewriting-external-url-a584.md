---
version: 2
name: cross-cutting-proxy-path-rewriting-external-url-a584
description: "Centralized Route Resolution via resolve-route Module: Proxy Path Rewriting External Url Construction"
metadata:
  actual:
    adr_id: a584f935-9c3c-44ae-afdf-fcc16116cf83
    status: accepted
    candidate_group_id: c1192f46-e2dc-4500-93bd-53907a2f23e6
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/lib/proxy/**
---

# Centralized Route Resolution via resolve-route Module: Proxy Path Rewriting External Url Construction

Scope: applies to `apps/status-page/src/lib/proxy/**`.

These rules are ALWAYS ACTIVE for proxy request handling, URL construction, locale override resolution, and path prefix normalization routines.

### Rules

- **R-PROXY-001** MUST: All proxy path rewriting, external URL construction, and locale override logic MUST resolve routes through the resolve-route module.

### Verify

```bash
# Discover workspace static analysis, linting, type checking, and test scripts from the dependency manifest and execute them
npm run lint
npm run typecheck
npm test
```

**Accept when:**
- All proxy path transformation functions import and route path parsing through the resolve-route module.
- Repository test suites and type verification scripts pass without route resolution errors.

<enforcement>
Automated continuous integration checks executing repository test suites and static analysis verify compliance. Pull requests introducing ad-hoc path parsing in proxy handlers will fail review and must be refactored to use resolve-route.
</enforcement>