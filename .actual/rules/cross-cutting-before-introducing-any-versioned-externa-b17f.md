---
version: 2
name: cross-cutting-before-introducing-any-versioned-externa-b17f
description: "In-Memory Key-Value Mapping Pattern in Importer Providers: Before Introducing Any Versioned External Datastore"
metadata:
  actual:
    adr_id: b17fcf25-72fd-4618-9dab-ce69448375ff
    status: accepted
    candidate_group_id: b7728fd2-3ad8-4ede-ba50-a11f197b9682
    evidence:
      source: deep
  scope:
    root: packages/importers
    paths:
      - src/providers/**
---

# In-Memory Key-Value Mapping Pattern in Importer Providers: Before Introducing Any Versioned External Datastore

Scope: applies to `packages/importers/src/providers/**`.

These rules are ALWAYS ACTIVE for all entity mapping routines within data ingestion providers requiring identifier resolution across related data structures.

### Rules

- **R-IMP-001** MUST: Before introducing any versioned external datastore or caching dependency, consumers MUST discover the repository lock artifact and verify the exact resolved version against authorized dependency policies.
- **R-IMP-002** MANDATORY LOCK-VERSION GROUNDING: Before writing code that uses a versioned library, execute in order: (1) Find the dependency manifest in the repo declaring ranges; (2) Identify the build tool from the manifest; (3) Inspect the repository lock or resolution artifact to determine the exact resolved version; (4) Look up official documentation/changelog/public API reference for that exact version via public internet search; (5) Confirm every API, class, or function called exists in that exact version; (6) Re-run steps 3-5 per dependency at point of use for version-sensitive behavior.
- **R-IMP-003** MUST: Ensure that lookup structures are instantiated per import execution and dereferenced upon mapper completion to enable garbage collection.
- **R-IMP-004** MUST: Normalize identifier keys to consistent string representations prior to insertion and retrieval to prevent type-mismatch misses.

### Verify

```bash
# Discover and execute project unit and integration test suites validating provider mapping routines and identifier resolution behavior
# Discover and run static analysis and linting scripts to verify absence of undeclared datastore dependencies within provider modules
```

**Accept when:**
- All provider mapping test suites pass with complete resolution of related entity identifiers.
- Static analysis confirms no persistent datastore connections or unauthorized external cache dependencies are imported in provider mapping modules.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated test suites in CI validating mapper transformations and code reviews ensuring in-memory lookup scopes remain confined to ingestion lifecycles.
</enforcement>