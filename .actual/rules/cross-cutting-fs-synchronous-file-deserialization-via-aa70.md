---
version: 2
name: cross-cutting-fs-synchronous-file-deserialization-via-aa70
description: "node:fs Synchronous File Deserialization via JSON.parse: Build Scripts Executing File Ingestion Via"
metadata:
  actual:
    adr_id: aa7035d6-2b9d-4896-90d1-dcbbff03b34a
    status: accepted
    candidate_group_id: 896a6963-08d0-4e2c-8aae-65bf89cf6ad6
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - scripts/**
---

# node:fs Synchronous File Deserialization via JSON.parse: Build Scripts Executing File Ingestion Via

Scope: applies to `packages/ui/scripts/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-FS-001** MUST: Build scripts executing file ingestion via node:fs readFileSync MUST wrap JSON.parse invocations in defensive error handling or validate incoming structures against defined constraints prior to accessing nested properties.

### Verify

```bash
# Discover repository validation scripts and execute the configured test suite
# Inspect project configuration to locate and run static analysis checks across build scripts
```

**Accept when:**
- All build scripts ingest configuration files without throwing unhandled deserialization exceptions.
- The repository verification suite executes without syntax or property access errors across transformed assets.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>