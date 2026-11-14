---
version: 2
name: cross-cutting-fs-synchronous-file-deserialization-via-6a30
description: "node:fs Synchronous File Deserialization via JSON.parse: Scripts Not Allow Unhandled Deserialization Failures"
metadata:
  actual:
    adr_id: 6a30d3b2-ea92-448f-9016-7fcc61124297
    status: accepted
    candidate_group_id: 896a6963-08d0-4e2c-8aae-65bf89cf6ad6
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - scripts/**
---

# node:fs Synchronous File Deserialization via JSON.parse: Scripts Not Allow Unhandled Deserialization Failures

Scope: applies to `packages/ui/scripts/**`.

These rules are ALWAYS ACTIVE for internal build and transformation scripts performing configuration file ingestion via runtime file system APIs.

### Rules

- **R-FS-001** MUST_NOT: Scripts MUST_NOT allow unhandled deserialization failures from JSON.parse to terminate execution without emitting diagnostic location details for the invalid configuration artifact.
- **R-FS-002** MANDATORY: DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.
- **R-FS-003** MANDATORY: LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
  1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
  2. Identify the build tool from the manifest.
  3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
  4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
  5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
  6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- **R-FS-004** MUST: Ensure file read operations using node:fs specify explicit text encoding prior to passing content to deserialization routines.
- **R-FS-005** MUST: Enclose JSON.parse calls in structured error-handling blocks to capture syntax exceptions and emit diagnostic context.

### Verify

```bash
# Discover repository validation scripts and execute the configured test suite
# Inspect project configuration to locate and run static analysis checks across build scripts
```

**Accept when:**
- All build scripts ingest configuration files without throwing unhandled deserialization exceptions.
- The repository verification suite executes without syntax or property access errors across transformed assets.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>