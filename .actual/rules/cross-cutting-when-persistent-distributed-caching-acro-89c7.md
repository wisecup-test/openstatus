---
version: 2
name: cross-cutting-when-persistent-distributed-caching-acro-89c7
description: "Standard Runtime Key-Value Mapping and Ephemeral State Access: When Persistent Distributed Caching Across Serverless"
metadata:
  actual:
    adr_id: 89c73f69-a89b-4f00-8f8c-cc61bbd80c34
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

# Standard Runtime Key-Value Mapping and Ephemeral State Access: When Persistent Distributed Caching Across Serverless

Scope: applies to `apps/web/src/app/**`, `apps/web/src/content/**`, `apps/web/src/lib/**`, `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for modules performing key-value retrieval, mutation, or caching operations across serverless and route handler boundaries.

### Rules

- **R-CACH-001** SHOULD: When persistent or distributed caching across serverless invocations is required, modules MUST adopt a standardized shared client rather than ad-hoc in-memory structures.
- **R-CACH-002** MANDATORY: Scope in-memory key-value data structures strictly to local function scopes to prevent unintended state sharing across asynchronous boundaries.
- **R-CACH-003** MANDATORY: When introducing external caching clients, ensure keys follow structured namespaces to prevent collision across independent application domains.
- **R-CACH-004** MANDATORY: Before writing code that uses a versioned library, execute the lock-version grounding sequence: find the dependency manifest, identify the build tool, inspect the repository lock or resolution artifact for the exact resolved version, look up official documentation for that exact version, confirm API existence, and re-verify per dependency at point of use.

### Verify

```bash
# Discover and run static analysis and type verification scripts
# Execute repository test suite to verify parameter handling and caching routines
```

**Accept when:**
- All static analysis and linting checks complete with zero errors.
- Unit and integration tests verify that transient state structures do not persist across isolated test cases.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests exhibiting unauthorized shared in-memory caching or unmanaged state retention are blocked from merging until corrected.
</enforcement>