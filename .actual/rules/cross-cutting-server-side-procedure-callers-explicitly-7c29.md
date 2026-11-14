---
version: 2
name: cross-cutting-server-side-procedure-callers-explicitly-7c29
description: "@trpc/client Library Adoption: Server Side Procedure Callers Explicitly Propagate"
metadata:
  actual:
    adr_id: 7c29a4b7-0f39-47fc-85e4-4ee42c9e56ef
    status: accepted
    candidate_group_id: 3057ad28-51be-466c-8491-81d8aa5079b9
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/trpc/**
---

# @trpc/client Library Adoption: Server Side Procedure Callers Explicitly Propagate

Scope: applies to `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for all web application client-side and server-side modules that initiate internal remote procedure calls to backend API procedures, and shared client communication configurations and procedure client boundary instantiations.

### Rules

- **R-TRPC-001** MUST: Server-side procedure callers MUST explicitly propagate source tracking headers while sanitizing hop-by-hop transport headers before invoking remote endpoints.

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