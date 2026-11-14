---
version: 2
name: cross-cutting-server-tier-entrypoints-not-reference-c46d
description: "Internal `./shared` Module Adoption for Isomorphic Client-Server Boundaries: Server Tier Entrypoints Not Reference Browser"
metadata:
  actual:
    adr_id: c46d271e-29a1-4a5b-83a8-a8db6c8a358a
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

# Internal `./shared` Module Adoption for Isomorphic Client-Server Boundaries: Server Tier Entrypoints Not Reference Browser

Scope: applies to `apps/web/src/lib/preferred-settings/**`, `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ADR-001** MUST_NOT: Server-tier entrypoints MUST NOT reference browser document models, client window globals, or reactive view lifecycle hooks.

### Verify

```bash
# Discover and run the project verification script from the repository manifest for module boundary and type checking
# Discover and run the repository test runner script for integration tests verifying client and server entrypoint isolation
# Discover and run the build analysis script for production bundle evaluation
```

**Accept when:**
- Static dependency analysis reports zero illegal cross-tier imports between client and server module entrypoints.
- All shared contracts and validation schemas compile cleanly without referencing environment-specific globals.
- Production client bundle inspection confirms complete absence of server-exclusive packages and header stores.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>