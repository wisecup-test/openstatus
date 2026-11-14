---
version: 2
name: cross-cutting-developers-not-treat-request-search-8db9
description: "Standard Runtime Key-Value Mapping and Ephemeral State Access: Developers Not Treat Request Search Parameter"
metadata:
  actual:
    adr_id: 8db9aebe-2db9-41aa-a288-20cf44bbec39
    status: accepted
    candidate_group_id: 012be8c2-fbee-4f98-a859-b7530f4838b8
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
      - src/content/**
      - src/lib/**
      - src/trpc/**
---

# Standard Runtime Key-Value Mapping and Ephemeral State Access: Developers Not Treat Request Search Parameter

Scope: applies to `apps/web/src/app/**`, `apps/web/src/content/**`, `apps/web/src/lib/**`, `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for all transient in-memory data associations, request query parameter lookups, and key-value mapping implementations within isolated execution boundaries.

### Rules

- **R-KV-001** MUST_NOT: Developers MUST NOT treat request search parameter accessors or local in-memory dictionaries as shared or persistent caching layers across execution boundaries.
- **R-KV-002** MANDATORY: Scope in-memory key-value data structures strictly to local function scopes to prevent unintended state sharing across asynchronous boundaries.
- **R-KV-003** MANDATORY: When introducing external caching clients, ensure keys follow structured namespaces to prevent collision across independent application domains.
- **R-KV-004** MANDATORY: Prior to writing code that uses a versioned library, execute the lock-version grounding process (find manifest, identify build tool, inspect lock/resolution artifact, lookup official documentation for exact version, confirm APIs exist, re-run per dependency at point of use).

### Verify

```bash
# Discover and run the project static analysis and type verification scripts
# Example (project-specific):
# npm run lint
# npm run typecheck
# npm test
```

**Accept when:**
- All static analysis and linting checks complete with zero errors.
- Unit and integration tests verify that transient state structures do not persist across isolated test cases.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks, peer code reviews, and test suites strictly enforce separation between local state and persistent caching.
</enforcement>