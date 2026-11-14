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

- **R-PROV-001** SHOULD: Providers populate identifier lookups during initial collection before invoking normalization mappers to prevent redundant iterations.
- **R-PROV-002** MANDATORY: The consumer MUST derive all tool names, file names, commands, package managers, and version numbers from the project repository (Discovery Policy).
- **R-PROV-003** MANDATORY: Before writing code that uses a versioned library, execute in order: find dependency manifest, identify build tool, inspect repository lock or resolution artifact for exact resolved version, look up official documentation for that exact version, confirm every API/class/function exists in that version, and re-run steps per dependency at point of use (Lock-Version Grounding).
- **R-PROV-004** MANDATORY: Ensure that lookup structures are instantiated per import execution and dereferenced upon mapper completion to enable garbage collection.
- **R-PROV-005** MANDATORY: Normalize identifier keys to consistent string representations prior to insertion and retrieval to prevent type-mismatch misses.

### Verify

```bash
# Discover and execute project unit and integration test suites validating provider mapping routines
# Discover and run static analysis and linting scripts to verify absence of undeclared datastore dependencies
```

**Accept when:**
- All provider mapping test suites pass with complete resolution of related entity identifiers.
- Static analysis confirms no persistent datastore connections or unauthorized external cache dependencies are imported in provider mapping modules.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated test suites in continuous integration validate mapper transformations, and code reviews ensure in-memory lookup scopes remain confined to ingestion lifecycles.
</enforcement>