---
version: 2
name: cross-cutting-route-search-parameter-schemas-encapsula-9984
description: "nuqs/server Adoption for Server-Side Search Parameter Parsing: Route Search Parameter Schemas Encapsulated Dedicated"
metadata:
  actual:
    adr_id: 9984475f-7af6-448b-960f-a3ebc8fde1d4
    status: accepted
    candidate_group_id: 0b22b5e0-eabb-47cb-98c4-c984563bd2c6
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
---

# nuqs/server Adoption for Server-Side Search Parameter Parsing: Route Search Parameter Schemas Encapsulated Dedicated

Scope: applies to `apps/web/src/app/**`.

These rules are ALWAYS ACTIVE for all server-side components, route entrypoints, and server loaders that ingest URL query parameters.

### Rules

- **R-NUQS-001** SHOULD: Route search parameter schemas SHOULD be encapsulated in dedicated parameter definition modules adjacent to their consuming route components.

### Verify

```bash
# Discover and execute the repository type-checking script to confirm server-side search parameter parsers comply with library type definitions
# Discover and run the project test suite to verify route parameter parsing and validation behaviors succeed
```

**Accept when:**
- Server-side search parameter declarations type-check cleanly against the library parser definitions.
- The repository verification suites execute without query-parsing or type mismatches.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests containing unvalidated query parameter access or manual string parsing in server routes will fail automated review gates.
</enforcement>