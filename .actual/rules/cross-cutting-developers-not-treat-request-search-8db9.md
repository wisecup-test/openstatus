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

These rules are ALWAYS ACTIVE for transient in-memory data associations and request query parameter lookups within isolated execution boundaries.

### Rules

- **R-MAP-001** MUST_NOT: Developers MUST NOT treat request search parameter accessors or local in-memory dictionaries as shared or persistent caching layers across execution boundaries.

### Verify

```bash
# Discover and run the project static analysis and type verification scripts
# Execute the repository test suite to ensure request parameter handling and caching routines pass all behavioral assertions
```

**Accept when:**
- All static analysis and linting checks complete with zero errors.
- Unit and integration tests verify that transient state structures do not persist across isolated test cases.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration checks and peer code review.
</enforcement>