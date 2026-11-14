---
version: 2
name: cross-cutting-rate-limiting-operations-applied-public-20a5
description: "Adopt `@openstatus/upstash` for Distributed State and Rate Limiting: Rate Limiting Operations Applied Public Procedures"
metadata:
  actual:
    adr_id: 20a5744c-d78f-468c-9122-5d3d618930c3
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

# Adopt `@openstatus/upstash` for Distributed State and Rate Limiting: Rate Limiting Operations Applied Public Procedures

Scope: applies to `packages/api/src/router/**`, `packages/api/src/service/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-UPSTASH-001** MUST: Rate-limiting operations applied to public API procedures MUST extract client identifiers through forwarded network headers provided by the request context.
- **R-UPSTASH-002** MUST: Wrap all rate-limiting and distributed cache invocations in defensive error-handling logic that logs diagnostic warnings and enables graceful fallback.
- **R-UPSTASH-003** MUST: Derive rate-limiting client keys from request context headers rather than relying on ephemeral local socket addresses.

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