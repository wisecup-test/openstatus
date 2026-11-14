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

These rules are ALWAYS ACTIVE for all application modules, API route handlers, background tasks, and UI components handling geographic region identifiers, edge locations, or regional latency metrics.

### Rules

- **R-REG-001** MUST: Before writing code that consumes or extends region definitions, developers MUST inspect the repository dependency manifest and lock artifact to resolve the exact locked version of @openstatus/regions.
- **R-REG-002** MUST: Follow the lock-version grounding process (find manifest, identify build tool, inspect lock artifact, look up official documentation for that exact version, confirm APIs exist, and re-run for version-sensitive behavior) before using a versioned library.
- **R-REG-003** MUST: Inspect the workspace structure to identify exports provided by the shared region module before introducing new probe target schemas.
- **R-REG-004** MUST: Ensure all regional metric lookups and filtering operations consume the canonical region identifiers exported by the shared module.

### Verify

```bash
# Discover the test script in the project dependency manifest and execute the test suite covering regional schema validation and probe routing.
# Discover the type inspection and static analysis script in the workspace configuration and execute it to verify all region imports resolve without error.
```

**Accept when:**
- All test suites validating regional latency checks, probe routing, and payload parsing complete with zero schema validation errors.
- Static analysis and type validation confirm that all regional attributes and lookup functions resolve directly to @openstatus/regions without unresolved symbol errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated static analysis checks in the CI pipeline, schema validation tests, and peer code reviews.
</enforcement>