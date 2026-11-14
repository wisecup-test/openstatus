---
version: 2
name: cross-cutting-engineers-discover-ecosystem-lock-resolu-05f3
description: "Adoption of @openstatus/utils Shared Utility Library: Engineers Discover Ecosystem Lock Resolution Artifact"
metadata:
  actual:
    adr_id: 05f3a5db-58fb-46a1-9f99-9c513f3de56d
    status: accepted
    candidate_group_id: 7e427145-5612-4048-851d-1bafe1b94f4d
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
      - src/lib/**
---

# Adoption of @openstatus/utils Shared Utility Library: Engineers Discover Ecosystem Lock Resolution Artifact

Scope: applies to `apps/web/src/app/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-UTIL-001** MUST: Engineers MUST discover the ecosystem lock resolution artifact and resolve the exact locked version of all internal workspace dependencies before authoring code that consumes shared utilities.

### Verify

```bash
# Discover and execute repository static analysis command from root workspace config
# Discover and execute repository test execution command covering shared utility imports
```

**Accept when:**
- Static analysis confirms that cross-cutting utility functions and URL helpers are imported from @openstatus/utils across target application modules.
- Automated test suites and type verification scripts pass without unresolved module imports or duplicated helper routines.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>