---
version: 2
name: cross-cutting-backend-components-requiring-unique-non-9bf3
description: "Adoption of nanoid for Unique Identifier Generation: Backend Components Requiring Unique Non Sequential"
metadata:
  actual:
    adr_id: 9bf34019-ce8d-41bd-bea5-056ed648b53e
    status: accepted
    candidate_group_id: a6d728a0-06cf-4329-9a4d-a16127e3c802
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Adoption of nanoid for Unique Identifier Generation: Backend Components Requiring Unique Non Sequential

Scope: applies to `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for backend components requiring unique, non-sequential identifier generation, asynchronous workflows, confirmation tokens, state keys, and RPC interceptors.

### Rules

- **R-NANO-001** MUST: Backend components requiring unique, non-sequential identifier generation MUST use nanoid as the standard identifier generation library.

### Verify

```bash
# Discover repository build configuration and execute test suite
# (Execute project-specific test, lint, and type check commands discovered from the repository manifest)
```

**Accept when:**
- All unit and integration tests exercising asynchronous action stores and RPC interceptors pass without identifier generation errors.
- Static type verification confirms nanoid is imported and called in accordance with its resolved library type definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>