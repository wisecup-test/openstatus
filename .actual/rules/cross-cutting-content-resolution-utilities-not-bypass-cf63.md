---
version: 2
name: cross-cutting-content-resolution-utilities-not-bypass-cf63
description: "Internal Docs Module Architecture for Content Resolution and Listing: Content Resolution Utilities Not Bypass Docs"
metadata:
  actual:
    adr_id: cf634c56-de25-4564-85d6-41455bcad97a
    status: accepted
    candidate_group_id: b4a95d1d-deaf-41fb-a6d5-d31fd971047b
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/content/utils/**
---

# Internal Docs Module Architecture for Content Resolution and Listing: Content Resolution Utilities Not Bypass Docs

Scope: applies to `apps/web/src/content/utils/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DOC-001** MUST_NOT: Content resolution utilities MUST NOT bypass the docs module to perform direct or unvalidated filesystem reads for entity retrieval.

### Verify

```bash
# Discover and run the project verification script from the root manifest for static type checking across content utility modules
# Discover and execute test suites targeting content resolution and listing utilities via the project test runner
```

**Accept when:**
- Static type analysis passes with zero errors across all content utility modules.
- Automated test suites pass, verifying that entity lookup functions return matching items by slug and handle non-existent slugs without unhandled exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated continuous integration and peer code review.
</enforcement>