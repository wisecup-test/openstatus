---
version: 2
name: cross-cutting-server-side-page-route-entry-c6a0
description: "Adoption of nuqs for Type-Safe Server-Side Search Parameter Parsing: Server Side Page Route Entry Points"
metadata:
  actual:
    adr_id: c6a09138-1646-466c-b921-cd32e12609e4
    status: accepted
    candidate_group_id: d25c74b8-2b62-4f95-8d16-03b44b81995c
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/app/**
---

# Adoption of nuqs for Type-Safe Server-Side Search Parameter Parsing: Server Side Page Route Entry Points

Scope: applies to `apps/dashboard/src/app/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NUQS-001** MUST: Server-side page route entry points that accept URL query parameters MUST validate and parse incoming search parameters using searchParamsCache.parse from the nuqs library.

### Verify

```bash
# Discover and run static analysis, type checking, and test suites to verify nuqs search parameter parsing adherence
# (Commands to be discovered from the project repository package manifest)
npm run lint
npm run typecheck
npm test
```

**Accept when:**
- All server-side page routes parsing URL query parameters execute searchParamsCache.parse without type errors.
- Automated static analysis and type checking scripts complete with zero violations across all route modules.
- Unit and integration test suites validating search parameter parsing pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>