---
version: 2
name: cross-cutting-callers-invoking-rate-limiting-distribut-730f
description: "Adopt `@openstatus/upstash` for Distributed State and Rate Limiting: Callers Invoking Rate Limiting Distributed Cache"
metadata:
  actual:
    adr_id: 730fcb6f-022d-45b5-9724-0cd0c9474c83
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

# Adopt `@openstatus/upstash` for Distributed State and Rate Limiting: Callers Invoking Rate Limiting Distributed Cache

Scope: applies to `packages/api/src/router/**`, `packages/api/src/service/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-UPSTASH-001** SHOULD: Callers invoking rate-limiting and distributed cache operations through the shared module SHOULD implement non-fatal error trapping and diagnostic logging to ensure degradation resilience.

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