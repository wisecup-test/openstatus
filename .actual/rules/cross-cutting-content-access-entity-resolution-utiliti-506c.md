---
version: 2
name: cross-cutting-content-access-entity-resolution-utiliti-506c
description: "Internal Docs Module Architecture for Content Resolution and Listing: Content Access Entity Resolution Utilities Import"
metadata:
  actual:
    adr_id: 506c796a-8602-4d9f-bab9-1bbce767e124
    status: accepted
    candidate_group_id: b4a95d1d-deaf-41fb-a6d5-d31fd971047b
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/content/utils/**
---

# Internal Docs Module Architecture for Content Resolution and Listing: Content Access Entity Resolution Utilities Import

Scope: applies to `apps/web/src/content/utils/**`.

These rules are ALWAYS ACTIVE for all content utility modules and services responsible for resolving, listing, and reading document collections.

### Rules

- **R-DOC-001** MUST: Content access and entity resolution utilities MUST import and route all document collection lookups through the internal docs module.

### Verify

```bash
# Discover and run static type checking across content utility modules
# Discover and run the project test runner for content resolution and listing tests
```

**Accept when:**
- Static type analysis passes with zero errors across all content utility modules.
- Automated test suites pass, verifying that entity lookup functions return matching items by slug and handle non-existent slugs without unhandled exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>