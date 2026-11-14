---
version: 2
name: cross-cutting-before-writing-code-that-consumes-c777
description: "Adoption of @openstatus/regions for Unified Geographic Region Modeling: Before Writing Code That Consumes Extends"
metadata:
  actual:
    adr_id: c7777101-936c-48ab-9ae9-cb2dc8652df4
    status: accepted
    candidate_group_id: 903339f8-5d24-42ec-828d-2745bb17f92b
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
      - src/content/**
      - src/data/**
      - src/lib/checker/**
---

# Adoption of @openstatus/regions for Unified Geographic Region Modeling: Before Writing Code That Consumes Extends

Scope: applies to `apps/web/src/app/**`, `apps/web/src/content/**`, `apps/web/src/data/**`, `apps/web/src/lib/checker/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-REG-001** MUST: Before writing code that consumes or extends region definitions, developers MUST inspect the repository dependency manifest and lock artifact to resolve the exact locked version of @openstatus/regions.

### Verify

```bash
# Discover the test script in the project dependency manifest and execute the test suite covering regional schema validation and probe routing.
pnpm test
# Discover the type inspection and static analysis script in the workspace configuration and execute it to verify all region imports resolve without error.
pnpm check
```

**Accept when:**
- All test suites validating regional latency checks, probe routing, and payload parsing complete with zero schema validation errors.
- Static analysis and type validation confirm that all regional attributes and lookup functions resolve directly to @openstatus/regions without unresolved symbol errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>