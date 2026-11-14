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

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-PROXY-001** MUST: All proxy path rewriting, external URL construction, and locale override logic MUST resolve routes through the resolve-route module.

### Verify

```bash
# Discover the workspace static analysis, linting, type checking, and test scripts from the dependency manifest
# Example discovery & execution pattern:
# npm run lint
# npm run typecheck
# npm test
```

**Accept when:**
- All proxy path transformation functions import and route path parsing through the resolve-route module.
- Repository test suites and type verification scripts pass without route resolution errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks and mandatory peer code reviews enforce compliance; pull requests introducing ad-hoc path parsing in proxy handlers will fail review and must be refactored to use resolve-route.
</enforcement>