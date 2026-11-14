---
version: 2
name: cross-cutting-domain-transformation-functions-within-s-d598
description: "Adoption of Shared checker/utils Internal Module: Domain Transformation Functions Within Shared Checker"
metadata:
  actual:
    adr_id: d5981b9b-fe99-4c21-bfd0-1e8451db8988
    status: accepted
    candidate_group_id: 6419eda7-589c-4d1a-b9ae-286e9010add2
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
---

# Adoption of Shared checker/utils Internal Module: Domain Transformation Functions Within Shared Checker

Scope: applies to `apps/web/src/app/**`.

These rules are ALWAYS ACTIVE for all features, presentation components, and endpoints that process, format, or present checker domain data.

### Rules

- **R-CHV-001** SHOULD: Domain transformation functions within the shared checker/utils module SHOULD remain pure and side-effect-free to ensure interoperability across client and server runtimes.

### Verify

```bash
# Discover and run the project test runner across all test suites covering shared domain utilities.
# Discover and execute the repository type checker and linter to confirm import boundaries and type consistency across modules.
```

**Accept when:**
- All unit tests covering shared domain utility functions pass with zero errors.
- Static type analysis and lint checks complete without type mismatches or boundary violations across presentation and API routes.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>