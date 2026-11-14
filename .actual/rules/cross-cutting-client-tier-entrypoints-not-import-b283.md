---
version: 2
name: cross-cutting-client-tier-entrypoints-not-import-b283
description: "Internal `./shared` Module Adoption for Isomorphic Client-Server Boundaries: Client Tier Entrypoints Not Import Server"
metadata:
  actual:
    adr_id: b2837526-d31c-4565-b7d8-2756ccc93b23
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

# Internal `./shared` Module Adoption for Isomorphic Client-Server Boundaries: Client Tier Entrypoints Not Import Server

Scope: applies to `apps/web/src/lib/preferred-settings/**`, `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for all subsystems and library modules in the web application that provide functionality to both browser and server runtime contexts, and codebases requiring shared data schemas, type contracts, and protocol constants across isomorphic network boundaries.

### Rules

- **R-ISOC-001** MUST_NOT: Client-tier entrypoints MUST NOT import server runtime headers, server-exclusive cache stores, or server-only SDK dependencies.

### Verify

```bash
# Discover and execute project verification script for module boundary and type checking
npm run lint || pnpm lint || yarn lint
# Discover and execute integration tests verifying client and server entrypoint isolation
npm run test || pnpm test || yarn test
# Discover and execute build analysis script for production bundle evaluation
npm run build || pnpm build || yarn build
```

**Accept when:**
- Static dependency analysis reports zero illegal cross-tier imports between client and server module entrypoints.
- All shared contracts and validation schemas compile cleanly without referencing environment-specific globals.
- Production client bundle inspection confirms complete absence of server-exclusive packages and header stores.

<enforcement>
Claude Code MUST NOT skip or defer verification. Violation handling: Continuous integration builds fail upon detection of unauthorized cross-tier imports or environment leaks into `./shared`.
</enforcement>