---
version: 2
name: cross-cutting-route-definitions-specify-default-fallba-7221
description: "nuqs/server Adoption for Server-Side Search Parameter Parsing: Route Definitions Specify Default Fallback Values"
metadata:
  actual:
    adr_id: 72212cd4-55f8-4427-b1e4-946b921801bb
    status: accepted
    candidate_group_id: 0b22b5e0-eabb-47cb-98c4-c984563bd2c6
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
---

# nuqs/server Adoption for Server-Side Search Parameter Parsing: Route Definitions Specify Default Fallback Values

Scope: applies to `apps/web/src/app/**`.

These rules are ALWAYS ACTIVE for all server-side components, route entrypoints, and server loaders that ingest URL query parameters.

### Rules

- **R-NS-001** MAY: Route definitions MAY specify default fallback values within parser configurations to ensure deterministic server rendering when query parameters are omitted.

### Verify

```bash
# Discover and execute the repository type-checking script to confirm type definitions
# Discover and run the project test suite
```

**Accept when:**
- Server-side search parameter declarations type-check cleanly against the library parser definitions.
- The repository verification suites execute without query-parsing or type mismatches.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration type checking, static analysis, and peer review during pull request evaluation.
</enforcement>