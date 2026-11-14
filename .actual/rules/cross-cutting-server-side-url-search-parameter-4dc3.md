---
version: 2
name: cross-cutting-server-side-url-search-parameter-4dc3
description: "nuqs/server Adoption for Server-Side Search Parameter Parsing: Server Side Url Search Parameter Parsing"
metadata:
  actual:
    adr_id: 4dc3dac4-2224-413c-9428-fc218c4a6020
    status: accepted
    candidate_group_id: 0b22b5e0-eabb-47cb-98c4-c984563bd2c6
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
---

# nuqs/server Adoption for Server-Side Search Parameter Parsing: Server Side Url Search Parameter Parsing

Scope: applies to `apps/web/src/app/**`.

These rules are ALWAYS ACTIVE for all server-side components, route entrypoints, and server loaders that ingest URL query parameters.

### Rules

- **R-NUQS-001** MUST: All server-side URL search parameter parsing and type coercion MUST be executed using nuqs/server parser definitions rather than unvalidated manual string operations.

### Verify

```bash
# Discover and execute the repository type-checking script to confirm that all server-side search parameter parsers comply with library type definitions.
# Discover and run the project test suite to verify that route parameter parsing and validation behaviors succeed.
```

**Accept when:**
- Server-side search parameter declarations type-check cleanly against the library parser definitions.
- The repository verification suites execute without query-parsing or type mismatches.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>