---
version: 2
name: cross-cutting-developers-discover-project-dependency-m-4bee
description: "Internal `./shared` Module Adoption for Isomorphic Client-Server Boundaries: Developers Discover Project Dependency Manifest Resolve"
metadata:
  actual:
    adr_id: 4bee9ba5-fbc4-408e-89a7-da7e568fd6f0
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

# Internal `./shared` Module Adoption for Isomorphic Client-Server Boundaries: Developers Discover Project Dependency Manifest Resolve

Scope: applies to `apps/web/src/lib/preferred-settings/**`, `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ADR-001** MUST: Developers MUST discover the project dependency manifest and resolve the exact locked version of all ecosystem dependencies from the repository resolution artifact before implementation.

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