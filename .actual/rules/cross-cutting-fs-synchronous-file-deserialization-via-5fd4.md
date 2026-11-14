---
version: 2
name: cross-cutting-fs-synchronous-file-deserialization-via-5fd4
description: "node:fs Synchronous File Deserialization via JSON.parse: Developers Decouple File System Reading Via"
metadata:
  actual:
    adr_id: 5fd4f018-08ee-4b6c-b8ee-6986293ee32e
    status: accepted
    candidate_group_id: 896a6963-08d0-4e2c-8aae-65bf89cf6ad6
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - scripts/**
---

# node:fs Synchronous File Deserialization via JSON.parse: Developers Decouple File System Reading Via

Scope: applies to `packages/ui/scripts/**`.

These rules are ALWAYS ACTIVE for internal build and transformation scripts performing configuration file ingestion via runtime file system APIs.

### Rules

- **R-FS-001** SHOULD: Developers SHOULD decouple file system reading via node:fs from business logic by routing raw string data through a dedicated parsing utility.
- **R-FS-002** MANDATORY: Ensure file read operations using node:fs specify explicit text encoding prior to passing content to deserialization routines.
- **R-FS-003** MANDATORY: Enclose JSON.parse calls in structured error-handling blocks to capture syntax exceptions and emit diagnostic context.
- **R-FS-004** MANDATORY (DISCOVERY POLICY): The consumer MUST discover tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-FS-005** MANDATORY (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, find the dependency manifest, identify the build tool, inspect the resolution artifact for the exact version, look up official documentation for that exact version, and confirm API existence.

### Verify

```bash
# Discover repository validation scripts and execute the configured test suite
# Inspect project configuration to locate and run static analysis checks across build scripts
```

**Accept when:**
- All build scripts ingest configuration files without throwing unhandled deserialization exceptions.
- The repository verification suite executes without syntax or property access errors across transformed assets.

<enforcement>
Claude Code MUST NOT skip or defer verification. Pull requests introducing unhandled deserialization routines in build scripts are blocked until defensive error handling is added.
</enforcement>