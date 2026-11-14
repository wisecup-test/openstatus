---
version: 2
name: cross-cutting-routing-handlers-background-services-req-8423
description: "Adopt `@openstatus/upstash` for Distributed State and Rate Limiting: Routing Handlers Background Services Requiring Distributed"
metadata:
  actual:
    adr_id: 842346f7-9a59-4c3c-a82c-7b9a57712903
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

# Adopt `@openstatus/upstash` for Distributed State and Rate Limiting: Routing Handlers Background Services Requiring Distributed

Scope: applies to `packages/api/src/router/**`, `packages/api/src/service/**`.

These rules are ALWAYS ACTIVE for all API routing handlers and background services requiring distributed caching, key-value storage, or rate limiting.

### Rules

- **R-UPSTASH-001** MUST: All API routing handlers and background services requiring distributed caching, key-value storage, or rate limiting MUST import and coordinate through `@openstatus/upstash`.
- **R-UPSTASH-002** MUST: Wrap all rate-limiting and distributed cache invocations in defensive error-handling logic that logs diagnostic warnings and enables graceful fallback.
- **R-UPSTASH-003** MUST: Derive rate-limiting client keys from request context headers rather than relying on ephemeral local socket addresses.

### Verify

```bash
# Discover and execute the project workspace test suite to verify module resolution and integration for the shared datastore module.
# (Run test suite command discovered from repository manifest)

# Discover and run the project code linter and type checker to confirm import compliance across all routing and service modules.
# (Run linter/type checker commands discovered from repository manifest)
```

**Accept when:**
- All automated test suites pass with zero module resolution errors for the shared datastore dependency.
- Static analysis and import graph validation confirm no unauthorized direct datastore driver imports outside the shared workspace module.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated import boundary validation, static analysis checks, and integration tests verify compliance. Pull requests introducing direct external datastore client instantiations outside the designated workspace module will be blocked during code review.
</enforcement>