---
version: 2
name: cross-cutting-developers-inspect-repository-lock-artif-a1c7
description: "Internal Docs Module Architecture for Content Resolution and Listing: Developers Inspect Repository Lock Artifact Verify"
metadata:
  actual:
    adr_id: a1c7126b-8235-4440-9fb9-451c92f7cdbb
    status: accepted
    candidate_group_id: b4a95d1d-deaf-41fb-a6d5-d31fd971047b
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/content/utils/**
---

# Internal Docs Module Architecture for Content Resolution and Listing: Developers Inspect Repository Lock Artifact Verify

Scope: applies to `apps/web/src/content/utils/**`.

These rules are ALWAYS ACTIVE for all content utility modules and services responsible for resolving, listing, and reading document collections.

### Rules

- **R-DOC-001** MUST: Developers MUST inspect the repository lock artifact to verify the exact resolved version of any external runtime or module dependencies before modifying content utilities.

### Verify

```bash
# Discover and run the project verification script for static type checking
# Discover and execute test suites targeting content resolution and listing utilities
```

**Accept when:**
- Static type analysis passes with zero errors across all content utility modules.
- Automated test suites pass, verifying that entity lookup functions return matching items by slug and handle non-existent slugs without unhandled exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated continuous integration pipeline executing type checking and test suites, and peer code review.
</enforcement>