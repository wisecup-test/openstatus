---
version: 2
name: cross-cutting-proxy-handlers-provide-contextual-metada-b0d2
description: "Centralized Route Resolution via resolve-route Module: Proxy Handlers Provide Contextual Metadata Resolve"
metadata:
  actual:
    adr_id: b0d208f3-2d4a-42d7-8685-885727bad7f2
    status: accepted
    candidate_group_id: c1192f46-e2dc-4500-93bd-53907a2f23e6
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/lib/proxy/**
---

# Centralized Route Resolution via resolve-route Module: Proxy Handlers Provide Contextual Metadata Resolve

Scope: applies to `apps/status-page/src/lib/proxy/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-PROXY-001** MAY: Proxy handlers MAY provide contextual metadata to resolve-route when evaluating tenant-specific or locale-specific route variations.

### Verify

```bash
# Discover and run the workspace static analysis, linting, type checking, and test scripts from the dependency manifest
# Example discovery step:
# npm run lint
# npm run typecheck
# npm test
```

**Accept when:**
- All proxy path transformation functions import and route path parsing through the resolve-route module.
- Repository test suites and type verification scripts pass without route resolution errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks and mandatory peer code review verify compliance; pull requests introducing ad-hoc path parsing in proxy handlers will fail review and must be refactored to use resolve-route.
</enforcement>