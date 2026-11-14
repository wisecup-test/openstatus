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

These rules are ALWAYS ACTIVE for modules involving transient in-memory data associations, request query parameter lookups, and serverless caching implementations.

### Rules

- **R-KV-001** SHOULD: When persistent or distributed caching across serverless invocations is required, modules SHOULD adopt a standardized shared client rather than ad-hoc in-memory structures.
- **R-KV-002** MANDATORY: Scope in-memory key-value data structures strictly to local function scopes to prevent unintended state sharing across asynchronous boundaries.
- **R-KV-003** MANDATORY: When introducing external caching clients, ensure keys follow structured namespaces to prevent collision across independent application domains.
- **R-KV-004** MANDATORY (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, execute in order: (1) Find the dependency manifest; (2) Identify the build tool; (3) Inspect the repository lock or resolution artifact; (4) Look up official documentation or public API reference for that exact version; (5) Confirm every API, class, or function exists in that version; (6) Re-run steps 3-5 per dependency at point of use.

### Verify

```bash
# Discover and run the project static analysis and type verification scripts
# Execute the repository test suite to ensure request parameter handling and caching routines pass all behavioral assertions
```

**Accept when:**
- All static analysis and linting checks complete with zero errors.
- Unit and integration tests verify that transient state structures do not persist across isolated test cases.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks, peer code review, and block-on-violation policies apply.
</enforcement>