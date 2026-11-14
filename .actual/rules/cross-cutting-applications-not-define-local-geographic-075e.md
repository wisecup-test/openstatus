---
version: 2
name: cross-cutting-applications-not-define-local-geographic-075e
description: "Adoption of @openstatus/regions for Unified Geographic Region Modeling: Applications Not Define Local Geographic Region"
metadata:
  actual:
    adr_id: 075e6226-98d3-4431-900e-54d23a9e4269
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

# Adoption of @openstatus/regions for Unified Geographic Region Modeling: Applications Not Define Local Geographic Region

Scope: applies to `apps/web/src/app/**`, `apps/web/src/content/**`, `apps/web/src/data/**`, `apps/web/src/lib/checker/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-REG-001** MUST_NOT: Applications MUST_NOT define local geographic region enums, custom region identifier string unions, or duplicate region metadata outside of @openstatus/regions.

### Verify

```bash
# Discover and execute the test script from the project dependency manifest covering regional schema validation and probe routing
# Discover and execute the type inspection and static analysis script in the workspace configuration to verify all region imports resolve without error
```

**Accept when:**
- All test suites validating regional latency checks, probe routing, and payload parsing complete with zero schema validation errors.
- Static analysis and type validation confirm that all regional attributes and lookup functions resolve directly to @openstatus/regions without unresolved symbol errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. All regional attribute and lookup resolutions must be statically verified.
</enforcement>