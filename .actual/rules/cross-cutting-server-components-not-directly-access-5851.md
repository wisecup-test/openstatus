---
version: 2
name: cross-cutting-server-components-not-directly-access-5851
description: "nuqs/server Adoption for Server-Side Search Parameter Parsing: Server Components Not Directly Access Raw"
metadata:
  actual:
    adr_id: 585143c4-68ec-4f2e-a9ac-3f6d880e60d2
    status: accepted
    candidate_group_id: 0b22b5e0-eabb-47cb-98c4-c984563bd2c6
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
---

# nuqs/server Adoption for Server-Side Search Parameter Parsing: Server Components Not Directly Access Raw

Scope: applies to `apps/web/src/app/**`.

These rules are ALWAYS ACTIVE for all server-side components, route entrypoints, and server loaders that ingest URL query parameters.

### Rules

- **R-NUQS-001** MUST_NOT: Server components MUST_NOT directly access raw query dictionaries without passing them through validated library parsers.

### Verify

```bash
# Discover and execute the repository type-checking script to confirm server-side search parameter parsers comply with library type definitions
# Discover and run the project test suite to verify route parameter parsing and validation behaviors succeed
```

**Accept when:**
- Server-side search parameter declarations type-check cleanly against the library parser definitions.
- The repository verification suites execute without query-parsing or type mismatches.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated continuous integration type checking, static analysis checks, and peer review.
</enforcement>