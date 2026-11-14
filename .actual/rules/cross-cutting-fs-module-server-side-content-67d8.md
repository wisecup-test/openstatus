---
version: 2
name: cross-cutting-fs-module-server-side-content-67d8
description: "Adoption of node:fs Module for Server-Side Content and Asset Ingestion: Modules Performing Synchronous Configuration Metadata Loading"
metadata:
  actual:
    adr_id: 67d83faa-f037-493b-ac0a-97a8c34e2aff
    status: accepted
    candidate_group_id: c198f3a9-7863-4d87-84dc-8c48dbf3824f
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - next.config.ts
      - src/content/**
      - src/lib/**
---

# Adoption of node:fs Module for Server-Side Content and Asset Ingestion: Modules Performing Synchronous Configuration Metadata Loading

Scope: applies to `apps/web/next.config.ts`, `apps/web/src/content/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for server-side content ingestion routines, documentation readers, static asset metadata extractors, build-time configuration scripts, and build-phase artifact processors.

### Rules

- **R-NODEFS-001** MAY: Modules performing synchronous configuration or metadata loading during initialization MAY use readFileSync when asynchronous streaming introduces unnecessary coordination complexity.

### Verify

```bash
# Discover and run the project's static analysis and linting scripts to verify that node:fs imports are absent from client component directories.
# Discover and run the project's test suite to ensure that all server-side content reading and parsing utilities execute cleanly without filesystem errors.
# Execute the project's build validation routine to confirm that all build-time configuration and asset dimension extractors resolve local files successfully.
```

**Accept when:**
- Static analysis confirms that node:fs imports reside strictly within server-side utilities, build configuration, and server rendering components.
- All unit and integration tests for content reading, metadata validation, and asset dimension extraction pass without unhandled filesystem exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>