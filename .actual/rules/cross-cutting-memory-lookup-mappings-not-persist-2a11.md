---
version: 2
name: cross-cutting-memory-lookup-mappings-not-persist-2a11
description: "In-Memory Key-Value Mapping Pattern in Importer Providers: Memory Lookup Mappings Not Persist State"
metadata:
  actual:
    adr_id: 2a11386c-948f-445e-bcf6-7f83a2a0442d
    status: accepted
    candidate_group_id: b7728fd2-3ad8-4ede-ba50-a11f197b9682
    evidence:
      source: deep
  scope:
    root: packages/importers
    paths:
      - src/providers/**
---

# In-Memory Key-Value Mapping Pattern in Importer Providers: Memory Lookup Mappings Not Persist State

Scope: applies to `packages/importers/src/providers/**`.

These rules are ALWAYS ACTIVE for entity mapping routines within data ingestion providers requiring identifier resolution across related data structures.

### Rules

- **R-IM-001** MUST_NOT: In-memory lookup mappings MUST NOT persist state across independent ingestion runs or leak entity references outside the provider transformation boundary.
- **R-IM-002** MANDATORY: Lookup structures MUST be instantiated per import execution and dereferenced upon mapper completion to enable garbage collection.
- **R-IM-003** MANDATORY: Identifier keys MUST be normalized to consistent string representations prior to insertion and retrieval to prevent type-mismatch misses.

### Verify

```bash
# Discover and execute the project unit and integration test suites that validate provider mapping routines and identifier resolution behavior.
# Discover and run the static analysis and linting scripts to verify absence of undeclared datastore dependencies within provider modules.
```

**Accept when:**
- All provider mapping test suites pass with complete resolution of related entity identifiers.
- Static analysis confirms no persistent datastore connections or unauthorized external cache dependencies are imported in provider mapping modules.

<enforcement>
Claude Code MUST NOT skip or defer verification. All provider mapping test suites and static analysis checks must be executed and passed.
</enforcement>