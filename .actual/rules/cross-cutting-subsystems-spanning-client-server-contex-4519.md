---
version: 2
name: cross-cutting-subsystems-spanning-client-server-contex-4519
description: "Internal `./shared` Module Adoption for Isomorphic Client-Server Boundaries: Subsystems Spanning Client Server Contexts Separate"
metadata:
  actual:
    adr_id: 45190771-219c-4e1d-a1d4-04e5ea8638d0
    status: accepted
    candidate_group_id: a89df71c-b426-4d94-a699-ab4046839363
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/lib/preferred-settings/**
      - src/trpc/**
---

# Internal `./shared` Module Adoption for Isomorphic Client-Server Boundaries: Subsystems Spanning Client Server Contexts Separate

Scope: applies to `apps/web/src/lib/preferred-settings/**`, `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for subsystems and library modules in the web application that provide functionality to both browser and server runtime contexts, and codebases requiring shared data schemas, type contracts, and protocol constants across isomorphic network boundaries.

### Rules

- **R-SHARED-001** MUST: Subsystems spanning client and server contexts MUST separate execution logic into distinct client-tier and server-tier entrypoints that consume the internal `./shared` module independently.

### Verify

```bash
# Discover and execute project verification, testing, and build analysis scripts from the repository manifest
# to verify module boundaries, type checking, entrypoint isolation, and bundle contents.
```

**Accept when:**
- Static dependency analysis reports zero illegal cross-tier imports between client and server module entrypoints.
- All shared contracts and validation schemas compile cleanly without referencing environment-specific globals.
- Production client bundle inspection confirms complete absence of server-exclusive packages and header stores.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis and module boundary checks are enforced during continuous integration workflows.
</enforcement>