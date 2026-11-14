---
version: 2
name: cross-cutting-content-listing-operations-consume-confi-578d
description: "Internal Docs Module Architecture for Content Resolution and Listing: Content Listing Operations Consume Configuration Parameters"
metadata:
  actual:
    adr_id: 578d7872-1ef7-40d1-bbbb-f9c836669235
    status: accepted
    candidate_group_id: b4a95d1d-deaf-41fb-a6d5-d31fd971047b
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/content/utils/**
---

# Internal Docs Module Architecture for Content Resolution and Listing: Content Listing Operations Consume Configuration Parameters

Scope: applies to `apps/web/src/content/utils/**`.

These rules are ALWAYS ACTIVE for all content utility modules and services responsible for resolving, listing, and reading document collections.

### Rules

- **R-DOCS-001** SHOULD: Content listing operations SHOULD consume configuration parameters from the docs configuration module when enumerating collection manifests.

### Verify

```bash
# Discover and run the project verification script from the root manifest for static type checking across content utility modules
# Discover and run the test runner from repository configuration targeting content resolution and listing utilities
```

**Accept when:**
- Static type analysis passes with zero errors across all content utility modules.
- Automated test suites pass, verifying that entity lookup functions return matching items by slug and handle non-existent slugs without unhandled exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests containing direct filesystem reads or bypassed content lookups are blocked from merging.
</enforcement>