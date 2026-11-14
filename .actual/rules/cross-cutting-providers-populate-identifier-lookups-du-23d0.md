---
version: 2
name: cross-cutting-providers-populate-identifier-lookups-du-23d0
description: "In-Memory Key-Value Mapping Pattern in Importer Providers: Providers Populate Identifier Lookups During Initial"
metadata:
  actual:
    adr_id: 23d07ff6-1764-439f-b218-5b83423e0d30
    status: accepted
    candidate_group_id: b7728fd2-3ad8-4ede-ba50-a11f197b9682
    evidence:
      source: deep
  scope:
    root: packages/importers
    paths:
      - src/providers/**
---

# In-Memory Key-Value Mapping Pattern in Importer Providers: Providers Populate Identifier Lookups During Initial

Scope: applies to `packages/importers/src/providers/**`.

These rules are ALWAYS ACTIVE for entity mapping routines within data ingestion providers requiring identifier resolution across related data structures.

### Rules

- **R-PROVIDER-001** SHOULD: Providers SHOULD populate identifier lookups during initial collection before invoking normalization mappers to prevent redundant iterations.
- **R-PROVIDER-002** MANDATORY: Lookup structures MUST be instantiated per import execution and dereferenced upon mapper completion to enable garbage collection.
- **R-PROVIDER-003** MANDATORY: Normalize identifier keys to consistent string representations prior to insertion and retrieval to prevent type-mismatch misses.
- **R-PROVIDER-004** MANDATORY: The consumer MUST derive all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-PROVIDER-005** MANDATORY: Before writing code that uses a versioned library, execute in order: (1) Find dependency manifest in the repo, (2) Identify build tool, (3) Inspect repository lock/resolution artifact for exact resolved version, (4) Look up official docs for that exact version, (5) Confirm every API/class/function exists in that version, (6) Re-run steps 3-5 per dependency at point of use for version-sensitive behavior.

### Verify

```bash
# Discover and execute the project unit and integration test suites that validate provider mapping routines and identifier resolution behavior.
# Discover and run the static analysis and linting scripts to verify absence of undeclared datastore dependencies within provider modules.
```

**Accept when:**
- All provider mapping test suites pass with complete resolution of related entity identifiers.
- Static analysis confirms no persistent datastore connections or unauthorized external cache dependencies are imported in provider mapping modules.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated test suites and static analysis/code reviews enforce that in-memory lookup scopes remain confined to ingestion lifecycles and pull requests introducing persistent datastore calls or global mutable cache state are blocked.
</enforcement>