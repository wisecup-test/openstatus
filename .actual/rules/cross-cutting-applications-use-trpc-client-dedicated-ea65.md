---
version: 2
name: cross-cutting-applications-use-trpc-client-dedicated-ea65
description: "@trpc/client Library Adoption: Applications Use Trpc Client Dedicated Remote"
metadata:
  actual:
    adr_id: ea65941e-f0e8-426b-bf08-ab388d62c9af
    status: accepted
    candidate_group_id: 3057ad28-51be-466c-8491-81d8aa5079b9
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/trpc/**
---

# @trpc/client Library Adoption: Applications Use Trpc Client Dedicated Remote

Scope: applies to `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TRPC-001** MUST: Applications MUST use @trpc/client as the dedicated remote procedure call client library for communicating with backend procedures across client and server environments.

### Verify

```bash
# Discover and run the project static analysis and type verification script
# Discover and run the project test suite to validate procedure client request serialization and header sanitization behavior
```

**Accept when:**
- The static type verification passes with zero type errors across all client-side and server-side procedure invocation modules.
- Procedure client configurations correctly apply shared serialization transformers and propagate required source headers without hop-by-hop transport headers.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>