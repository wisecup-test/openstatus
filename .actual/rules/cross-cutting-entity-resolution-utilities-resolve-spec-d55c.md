---
version: 2
name: cross-cutting-entity-resolution-utilities-resolve-spec-d55c
description: "Internal Docs Module Architecture for Content Resolution and Listing: Entity Resolution Utilities Resolve Specific Content"
metadata:
  actual:
    adr_id: d55cbf87-01de-4438-bbb9-86b6afe2a248
    status: accepted
    candidate_group_id: b4a95d1d-deaf-41fb-a6d5-d31fd971047b
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/content/utils/**
---

# Internal Docs Module Architecture for Content Resolution and Listing: Entity Resolution Utilities Resolve Specific Content

Scope: applies to `apps/web/src/content/utils/**`.

These rules are ALWAYS ACTIVE for all content utility modules and services responsible for resolving, listing, and reading document collections.

### Rules

- **R-DOC-001** MUST: Entity resolution utilities MUST resolve specific content items by executing slug-equality predicate lookups against collection accessors exposed by the docs module.

### Verify

```bash
# Run static type checking across the content utility modules
# (Command to be discovered from the project root manifest)

# Execute test suites targeting content resolution and listing utilities
# (Command to be discovered from repository configuration)
```

**Accept when:**
- Static type analysis passes with zero errors across all content utility modules.
- Automated test suites pass, verifying that entity lookup functions return matching items by slug and handle non-existent slugs without unhandled exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is enforced via automated CI pipelines, peer code review, and PR blocking for direct filesystem reads or bypassed content lookups.
</enforcement>