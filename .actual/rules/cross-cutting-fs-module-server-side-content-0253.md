---
version: 2
name: cross-cutting-fs-module-server-side-content-0253
description: "Adoption of node:fs Module for Server-Side Content and Asset Ingestion: Consumers Inspect Repository Lock Artifact Runtime"
metadata:
  actual:
    adr_id: 02538d0e-4600-4e8c-b8f4-a5a9089a0742
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

# Adoption of node:fs Module for Server-Side Content and Asset Ingestion: Consumers Inspect Repository Lock Artifact Runtime

Scope: applies to `apps/web/next.config.ts`, `apps/web/src/content/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-FS-001** MUST: Consumers MUST inspect the repository lock artifact and runtime configuration to verify that the active execution runtime supports the node:fs protocol before introducing or modifying filesystem calls.

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