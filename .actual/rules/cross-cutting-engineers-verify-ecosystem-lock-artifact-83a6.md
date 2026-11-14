---
version: 2
name: cross-cutting-engineers-verify-ecosystem-lock-artifact-83a6
description: "@trpc/client Library Adoption: Engineers Verify Ecosystem Lock Artifact Before"
metadata:
  actual:
    adr_id: 83a6c263-6d05-4001-aa47-53ab5af3c161
    status: accepted
    candidate_group_id: 3057ad28-51be-466c-8491-81d8aa5079b9
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/trpc/**
---

# @trpc/client Library Adoption: Engineers Verify Ecosystem Lock Artifact Before

Scope: applies to `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for all web application client-side and server-side modules that initiate internal remote procedure calls to backend API procedures and shared client communication configurations and procedure client boundary instantiations.

### Rules

- **R-TRPC-001** MUST: Engineers MUST verify the ecosystem lock artifact before introducing or updating client procedure bindings to guarantee version alignment across client and server runtimes.

### Verify

```bash
# Discover and run the project static analysis and type verification script
# Discover and run the project test suite to validate procedure client request serialization and header sanitization behavior
npm run typecheck
npm test
```

**Accept when:**
- The static type verification passes with zero type errors across all client-side and server-side procedure invocation modules.
- Procedure client configurations correctly apply shared serialization transformers and propagate required source headers without hop-by-hop transport headers.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration type checking, static analysis workflows, and pull request code reviews enforce these rules.
</enforcement>