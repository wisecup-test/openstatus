---
version: 2
name: cross-cutting-shared-checker-utils-module-expose-382c
description: "Adoption of Shared checker/utils Internal Module: Shared Checker Utils Module Expose Modular"
metadata:
  actual:
    adr_id: 382c42d6-178b-40c3-8be4-16d14df96b80
    status: accepted
    candidate_group_id: 6419eda7-589c-4d1a-b9ae-286e9010add2
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
---

# Adoption of Shared checker/utils Internal Module: Shared Checker Utils Module Expose Modular

Scope: applies to `apps/web/src/app/**`.

These rules are ALWAYS ACTIVE for all features, presentation components, and endpoints that process, format, or present checker domain data.

### Rules

- **R-CHCK-001** MAY: The shared checker/utils module MAY expose modular subroutines to allow tree-shaking when consumed across different runtime contexts.

### Verify

```bash
# Discover and run the project test runner across all test suites covering shared domain utilities.
# Discover and execute the repository type checker and linter to confirm import boundaries and type consistency across modules.
```

**Accept when:**
- All unit tests covering shared domain utility functions pass with zero errors.
- Static type analysis and lint checks complete without type mismatches or boundary violations across presentation and API routes.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration checks covering unit tests, type analysis, and linting rules, and mandatory peer code review.
</enforcement>