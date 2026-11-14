---
version: 2
name: cross-cutting-collection-accessor-functions-maintain-c-9a84
description: "Internal Docs Module Architecture for Content Resolution and Listing: Collection Accessor Functions Maintain Consistent Naming"
metadata:
  actual:
    adr_id: 9a845c37-8975-4b91-a216-caf453086a42
    status: accepted
    candidate_group_id: b4a95d1d-deaf-41fb-a6d5-d31fd971047b
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/content/utils/**
---

# Internal Docs Module Architecture for Content Resolution and Listing: Collection Accessor Functions Maintain Consistent Naming

Scope: applies to `apps/web/src/content/utils/**`.

These rules are ALWAYS ACTIVE for all content utility modules and services responsible for resolving, listing, and reading document collections.

### Rules

- **R-DOC-001** SHOULD: Collection accessor functions SHOULD maintain consistent naming conventions across all structured content entity domains.

### Verify

```bash
# Discover the project verification script from the root manifest and run static type checking across the content utility modules.
# Discover the project test runner from repository configuration and execute test suites targeting content resolution and listing utilities.
```

**Accept when:**
- Static type analysis passes with zero errors across all content utility modules.
- Automated test suites pass, verifying that entity lookup functions return matching items by slug and handle non-existent slugs without unhandled exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated CI, code review, and build checks.
</enforcement>