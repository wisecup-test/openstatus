---
version: 2
name: cross-cutting-fs-module-server-side-content-c08c
description: "Adoption of node:fs Module for Server-Side Content and Asset Ingestion: Local Filesystem Reads Executed During Request"
metadata:
  actual:
    adr_id: c08c392d-5ced-4da0-9171-7e77a6737584
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

# Adoption of node:fs Module for Server-Side Content and Asset Ingestion: Local Filesystem Reads Executed During Request

Scope: applies to `apps/web/next.config.ts`, `apps/web/src/content/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-FS-001** SHOULD: Local filesystem reads executed during request handling or build operations SHOULD validate and parse retrieved file contents through schema validation or structured parsing prior to consumption.
- **R-FS-002** MANDATORY: All imports of the filesystem module MUST employ the node prefix protocol to enforce native runtime resolution.

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