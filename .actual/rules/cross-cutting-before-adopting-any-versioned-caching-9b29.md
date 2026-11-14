---
version: 2
name: cross-cutting-before-adopting-any-versioned-caching-9b29
description: "Standard Runtime Key-Value Mapping and Ephemeral State Access: Before Adopting Any Versioned Caching Dependency"
metadata:
  actual:
    adr_id: 9b29c3bd-7aeb-41db-a8d2-6fe39fe97a66
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

# Standard Runtime Key-Value Mapping and Ephemeral State Access: Before Adopting Any Versioned Caching Dependency

Scope: applies to `apps/web/src/app/**`, `apps/web/src/content/**`, `apps/web/src/lib/**`, `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for transient in-memory data associations and request query parameter lookups within isolated execution boundaries.

### Rules

- **R-CACHE-001** MUST: Before adopting any versioned caching dependency, the consumer MUST inspect the repository lock artifact to determine and verify the exact resolved version.
- **R-CACHE-002** MUST: Scope in-memory key-value data structures strictly to local function scopes to prevent unintended state sharing across asynchronous boundaries.
- **R-CACHE-003** MUST: When introducing external caching clients, ensure keys follow structured namespaces to prevent collision across independent application domains.

### Verify

```bash
# Discover and run the project static analysis and type verification scripts
# to verify that dictionary accessors conform to type definitions.

# Execute the repository test suite to ensure request parameter handling 
# and caching routines pass all behavioral assertions.
```

**Accept when:**
- All static analysis and linting checks complete with zero errors.
- Unit and integration tests verify that transient state structures do not persist across isolated test cases.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>