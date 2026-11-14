---
version: 2
name: cross-cutting-procedure-clients-capture-report-error-5239
description: "@trpc/client Library Adoption: Procedure Clients Capture Report Error Contexts"
metadata:
  actual:
    adr_id: 5239377e-f369-409c-b63d-646f0698264a
    status: accepted
    candidate_group_id: 3057ad28-51be-466c-8491-81d8aa5079b9
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/trpc/**
---

# @trpc/client Library Adoption: Procedure Clients Capture Report Error Contexts

Scope: applies to `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TRPC-001** SHOULD: Procedure clients SHOULD capture and report procedure error contexts through centralized observability handlers during request execution.

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