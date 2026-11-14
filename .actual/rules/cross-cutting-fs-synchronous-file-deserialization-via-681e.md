---
version: 2
name: cross-cutting-fs-synchronous-file-deserialization-via-681e
description: "node:fs Synchronous File Deserialization via JSON.parse: When Introducing External Schema Validation Libraries"
metadata:
  actual:
    adr_id: 681ed05e-d6a6-4014-93d7-0f60d6a91983
    status: accepted
    candidate_group_id: 896a6963-08d0-4e2c-8aae-65bf89cf6ad6
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - scripts/**
---

# node:fs Synchronous File Deserialization via JSON.parse: When Introducing External Schema Validation Libraries

Scope: applies to `packages/ui/scripts/**`.

These rules are ALWAYS ACTIVE for internal build and transformation scripts performing configuration file ingestion via runtime file system APIs.

### Rules

- **R-FS-001** MUST: When introducing external schema validation libraries to govern input parsing, developers MUST discover the ecosystem lock artifact and adhere to the resolved library version without relying on floating ranges.

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