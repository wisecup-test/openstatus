---
version: 2
name: cross-cutting-components-endpoints-not-bypass-shared-945f
description: "Adoption of Shared checker/utils Internal Module: Components Endpoints Not Bypass Shared Checker"
metadata:
  actual:
    adr_id: 945f657c-1868-41c2-83f6-5321b5653302
    status: accepted
    candidate_group_id: 6419eda7-589c-4d1a-b9ae-286e9010add2
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
---

# Adoption of Shared checker/utils Internal Module: Components Endpoints Not Bypass Shared Checker

Scope: applies to `apps/web/src/app/**`.

These rules are ALWAYS ACTIVE for all features, presentation components, and endpoints that process, format, or present checker domain data.

### Rules

- **R-CHK-001** MUST_NOT: Components and endpoints MUST_NOT bypass the shared checker/utils module by duplicating domain calculation, status formatting, or parsing algorithms.

### Verify

```bash
# Discover and run the project test runner across all test suites covering shared domain utilities
# Discover and execute the repository type checker and linter to confirm import boundaries and type consistency across modules
```

**Accept when:**
- All unit tests covering shared domain utility functions pass with zero errors.
- Static type analysis and lint checks complete without type mismatches or boundary violations across presentation and API routes.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration checks covering unit tests, type analysis, and linting rules, as well as mandatory peer code review.
</enforcement>