---
version: 2
name: cross-cutting-utility-functions-within-lib-utils-b490
description: "Adoption of @/lib/utils Internal Utility Module for Presentation Components: Utility Functions Within Lib Utils Module"
metadata:
  actual:
    adr_id: b4901a64-1ced-4048-ba06-ac9fc3b11b1c
    status: accepted
    candidate_group_id: f3dc5d23-ec5a-453d-880d-3f740a5b058d
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Adoption of @/lib/utils Internal Utility Module for Presentation Components: Utility Functions Within Lib Utils Module

Scope: applies to `apps/dashboard/src/components/**`.

These rules are ALWAYS ACTIVE for presentation tier components including data table cells, layout containers, and user interface elements that share utility logic across the application.

### Rules

- **R-UTIL-001** MUST_NOT: Utility functions within the @/lib/utils module MUST NOT introduce stateful side effects or direct dependencies on application routing context.

### Verify

```bash
# Discover and execute the repository type-checking script
# Discover and execute the repository test suite
# Discover and execute the repository linter
```

**Accept when:**
- Type checking completes with zero diagnostic errors across all components importing the utility module.
- All automated unit and regression tests for utility helpers and consuming components pass without failures.
- Linting checks confirm no duplicate local utility implementations or forbidden relative paths exist.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated continuous integration validation pipelines executing type checking, linting, and test suites on pull requests, alongside peer code review.
</enforcement>