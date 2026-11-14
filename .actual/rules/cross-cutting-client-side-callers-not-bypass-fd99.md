---
version: 2
name: cross-cutting-client-side-callers-not-bypass-fd99
description: "@trpc/client Library Adoption: Client Side Callers Not Bypass Shared"
metadata:
  actual:
    adr_id: fd999c70-a64b-4d85-bd45-f18baaee281f
    status: accepted
    candidate_group_id: 3057ad28-51be-466c-8491-81d8aa5079b9
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/trpc/**
---

# @trpc/client Library Adoption: Client Side Callers Not Bypass Shared

Scope: applies to `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TRPC-001** MUST_NOT: Client-side callers MUST NOT bypass the shared procedure client abstraction to issue raw unvalidated HTTP calls to internal procedure endpoints.

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