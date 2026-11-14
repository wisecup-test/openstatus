---
version: 2
name: cross-cutting-before-implementing-modifying-code-that-b868
description: "Adoption of @openstatus/icons and Brand Subpath for Centralized Iconography: Before Implementing Modifying Code That Depends"
metadata:
  actual:
    adr_id: b8687655-8564-4870-ab42-a17652fb0c32
    status: accepted
    candidate_group_id: 6657c8b1-ac0f-40f5-afa9-ba814c964fbd
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/app/login/**
      - src/components/**
      - src/config/**
      - src/data/**
---

# Adoption of @openstatus/icons and Brand Subpath for Centralized Iconography: Before Implementing Modifying Code That Depends

Scope: applies to `apps/dashboard/src/app/login/**`, `apps/dashboard/src/components/**`, `apps/dashboard/src/config/**`, `apps/dashboard/src/data/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ICN-001** MUST: Before implementing or modifying code that depends on the iconography library, developers MUST inspect the repository lock resolution artifact to determine the exact resolved version and exported module contracts.
- **R-ICN-002** MUST: Ensure icon component imports explicitly distinguish between general UI symbols from `@openstatus/icons` and third-party provider marks from `@openstatus/icons/brand`.
- **R-ICN-003** MUST: When introducing new visual marks, validate that stroke weights, viewports, and color inheritance attributes conform to existing workspace icon definitions.

### Verify

```bash
# Discover and execute the repository static analysis and type checking suite
# (Derive exact build tool and test runner from repository manifests)
npm run typecheck
npm run lint
npm run build
```

**Accept when:**
- All graphical icons and provider brand marks are imported exclusively through `@openstatus/icons` or `@openstatus/icons/brand`.
- Static analysis and type verification confirm clean symbol resolution across all user interface modules.
- Build artifact inspection verifies that icon subpaths bundle cleanly without duplicate vector definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification of static analysis, type checking, linting, and build bundling for icon assets is mandatory.
</enforcement>