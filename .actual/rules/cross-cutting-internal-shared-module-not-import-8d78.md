---
version: 2
name: cross-cutting-internal-shared-module-not-import-8d78
description: "Internal `./shared` Module Adoption for Isomorphic Client-Server Boundaries: Internal Shared Module Not Import Either"
metadata:
  actual:
    adr_id: 8d787cb1-2285-4b52-9534-1e5ce04fef1e
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

# Internal `./shared` Module Adoption for Isomorphic Client-Server Boundaries: Internal Shared Module Not Import Either

Scope: applies to `apps/web/src/lib/preferred-settings/**`, `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for subsystems and library modules in the web application that provide functionality to both browser and server runtime contexts, and codebases requiring shared data schemas, type contracts, and protocol constants across isomorphic network boundaries.

### Rules

- **R-ISOMORPHIC-001** MUST_NOT: The internal `./shared` module MUST NOT import from either the client-tier entrypoint or the server-tier entrypoint, maintaining strict unidirectional dependency flow.

### Verify

```bash
# Discover the project verification script from the repository manifest and execute module boundary and type checking.
# Discover the repository test runner script and execute integration tests verifying client and server entrypoint isolation.
# Discover the build analysis script and execute production bundle evaluation to ensure no server-side imports appear in client artifacts.
```

**Accept when:**
- Static dependency analysis reports zero illegal cross-tier imports between client and server module entrypoints.
- All shared contracts and validation schemas compile cleanly without referencing environment-specific globals.
- Production client bundle inspection confirms complete absence of server-exclusive packages and header stores.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>