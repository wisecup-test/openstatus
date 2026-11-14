---
version: 2
name: cross-cutting-components-rendering-regional-latency-ch-a68f
description: "Adoption of @openstatus/regions for Unified Geographic Region Modeling: Components Rendering Regional Latency Charts Diagnostic"
metadata:
  actual:
    adr_id: a68f2d62-0e26-4caa-80f6-e6c6d0cf4064
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

# Adoption of @openstatus/regions for Unified Geographic Region Modeling: Components Rendering Regional Latency Charts Diagnostic

Scope: applies to `apps/web/src/app/**`, `apps/web/src/content/**`, `apps/web/src/data/**`, `apps/web/src/lib/checker/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-REG-001** SHOULD: Components rendering regional latency charts and diagnostic tables SHOULD access region metadata, display labels, and ordering directly through @openstatus/regions helpers.

### Verify

```bash
# Discover and execute the test script covering regional schema validation and probe routing from package.json
# Discover and execute the type inspection/static analysis script from workspace configuration
```

**Accept when:**
- All test suites validating regional latency checks, probe routing, and payload parsing complete with zero schema validation errors.
- Static analysis and type validation confirm that all regional attributes and lookup functions resolve directly to @openstatus/regions without unresolved symbol errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated static analysis checks, schema validation tests, and code review.
</enforcement>