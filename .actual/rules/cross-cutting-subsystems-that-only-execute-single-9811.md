---
version: 2
name: cross-cutting-subsystems-that-only-execute-single-9811
description: "Internal `./shared` Module Adoption for Isomorphic Client-Server Boundaries: Subsystems That Only Execute Single Runtime"
metadata:
  actual:
    adr_id: 9811d927-dcd8-4ebb-a0de-f287632996e4
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

# Internal `./shared` Module Adoption for Isomorphic Client-Server Boundaries: Subsystems That Only Execute Single Runtime

Scope: applies to `apps/web/src/lib/preferred-settings/**`, `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for all subsystems and library modules in the web application that provide functionality to both browser and server runtime contexts, and codebases requiring shared data schemas, type contracts, and protocol constants across isomorphic network boundaries.

### Rules

- **R-SHARED-001** MAY: Subsystems that only execute in a single runtime environment MAY omit the tripartite entrypoint structure and export contracts directly from a unified entrypoint.

### Verify

```bash
# Discover and execute project verification, test, and build analysis scripts from the repository manifest
# to verify module boundary isolation, integration tests, and production bundle evaluation.
```

**Accept when:**
- Static dependency analysis reports zero illegal cross-tier imports between client and server module entrypoints.
- All shared contracts and validation schemas compile cleanly without referencing environment-specific globals.
- Production client bundle inspection confirms complete absence of server-exclusive packages and header stores.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis, boundary checks, and pull request reviews enforce adherence.
</enforcement>