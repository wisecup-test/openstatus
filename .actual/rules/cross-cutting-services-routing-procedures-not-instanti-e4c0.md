---
version: 2
name: cross-cutting-services-routing-procedures-not-instanti-e4c0
description: "Adopt `@openstatus/upstash` for Distributed State and Rate Limiting: Services Routing Procedures Not Instantiate External"
metadata:
  actual:
    adr_id: e4c07ad2-7040-40d2-8231-c5a355acd916
    status: accepted
    candidate_group_id: 50df5cbd-07a3-4e48-808a-e3b273ebd893
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/router/**
      - src/service/**
---

# Adopt `@openstatus/upstash` for Distributed State and Rate Limiting: Services Routing Procedures Not Instantiate External

Scope: applies to `packages/api/src/router/**`, `packages/api/src/service/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-UPSTASH-001** MUST_NOT: Services and routing procedures MUST NOT instantiate external distributed datastore or caching clients directly outside the designated shared workspace module.

### Verify

```bash
# Discover and execute the project workspace test suite to verify module resolution and integration for the shared datastore module.
# Discover and run the project code linter and type checker to confirm import compliance across all routing and service modules.
```

**Accept when:**
- All automated test suites pass with zero module resolution errors for the shared datastore dependency.
- Static analysis and import graph validation confirm no unauthorized direct datastore driver imports outside the shared workspace module.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>