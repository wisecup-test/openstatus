---
version: 2
name: cross-cutting-remote-procedure-call-client-configurati-c0a2
description: "@trpc/client Library Adoption: Remote Procedure Call Client Configurations Share"
metadata:
  actual:
    adr_id: c0a2681b-d260-49ad-91a7-2f6c03978694
    status: accepted
    candidate_group_id: 3057ad28-51be-466c-8491-81d8aa5079b9
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/trpc/**
---

# @trpc/client Library Adoption: Remote Procedure Call Client Configurations Share

Scope: applies to `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TRPC-001** MUST: Remote procedure call client configurations MUST share a unified serialization and error handling configuration across both client-side and server-side callers.

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