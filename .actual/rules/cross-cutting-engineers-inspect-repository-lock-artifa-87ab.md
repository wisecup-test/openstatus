---
version: 2
name: cross-cutting-engineers-inspect-repository-lock-artifa-87ab
description: "nuqs/server Adoption for Server-Side Search Parameter Parsing: Engineers Inspect Repository Lock Artifact Determine"
metadata:
  actual:
    adr_id: 87ab3554-cfab-4c0e-aaa1-afc0402eb90b
    status: accepted
    candidate_group_id: 0b22b5e0-eabb-47cb-98c4-c984563bd2c6
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
---

# nuqs/server Adoption for Server-Side Search Parameter Parsing: Engineers Inspect Repository Lock Artifact Determine

Scope: applies to `apps/web/src/app/**`.

These rules are ALWAYS ACTIVE for all server-side components, route entrypoints, and server loaders that ingest URL query parameters.

### Rules

- **R-NQS-001** MUST: Engineers MUST inspect the repository lock artifact to determine the exact resolved version of the query state library before authoring or modifying search parameter logic.

### Verify

```bash
# Discover and execute the repository type-checking script to confirm that all server-side search parameter parsers comply with library type definitions.
# Discover and run the project test suite to verify that route parameter parsing and validation behaviors succeed.
```

**Accept when:**
- Server-side search parameter declarations type-check cleanly against the library parser definitions.
- The repository verification suites execute without query-parsing or type mismatches.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>