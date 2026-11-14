---
version: 2
name: cross-cutting-checker-presentation-components-opengrap-f807
description: "Adoption of Shared checker/utils Internal Module: Checker Presentation Components Opengraph Image Synthesis"
metadata:
  actual:
    adr_id: f80757b8-6f6e-4a7d-b0ef-60be6106bb46
    status: accepted
    candidate_group_id: 6419eda7-589c-4d1a-b9ae-286e9010add2
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
---

# Adoption of Shared checker/utils Internal Module: Checker Presentation Components Opengraph Image Synthesis

Scope: applies to `apps/web/src/app/**`.

These rules are ALWAYS ACTIVE for all features, presentation components, and endpoints that process, format, or present checker domain data.

### Rules

- **R-CHECKER-001** MUST: Checker presentation components and OpenGraph image synthesis endpoints MUST import shared checker domain logic exclusively from the shared checker/utils internal module rather than reimplementing domain transformations locally.

### Verify

```bash
# Discover and run the project test runner across all test suites covering shared domain utilities.
# Discover and execute the repository type checker and linter to confirm import boundaries and type consistency across modules.
```

**Accept when:**
- All unit tests covering shared domain utility functions pass with zero errors.
- Static type analysis and lint checks complete without type mismatches or boundary violations across presentation and API routes.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration checks and mandatory peer code review.
</enforcement>