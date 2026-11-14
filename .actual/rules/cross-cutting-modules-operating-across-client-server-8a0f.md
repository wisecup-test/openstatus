---
version: 2
name: cross-cutting-modules-operating-across-client-server-8a0f
description: "Internal `./shared` Module Adoption for Isomorphic Client-Server Boundaries: Modules Operating Across Client Server Execution"
metadata:
  actual:
    adr_id: 8a0f6c15-e66f-4681-9897-ef999f67758c
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

# Internal `./shared` Module Adoption for Isomorphic Client-Server Boundaries: Modules Operating Across Client Server Execution

Scope: applies to `apps/web/src/lib/preferred-settings/**`, `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for subsystems and library modules in the web application that provide functionality to both browser and server runtime contexts, and codebases requiring shared data schemas, type contracts, and protocol constants across isomorphic network boundaries.

### Rules

- **R-SHARED-001** MUST: Modules operating across client and server execution tiers MUST encapsulate runtime-agnostic schemas, types, and constants within an internal `./shared` module rather than in environment-specific entrypoints.

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
Claude Code MUST NOT skip or defer verification. Verification is mandatory and enforced via automated static analysis, module boundary checks, and pull request architectural reviews.
</enforcement>