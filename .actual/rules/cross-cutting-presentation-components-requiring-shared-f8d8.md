---
version: 2
name: cross-cutting-presentation-components-requiring-shared-f8d8
description: "Adoption of @/lib/utils Internal Utility Module for Presentation Components: Presentation Components Requiring Shared Utility Functions"
metadata:
  actual:
    adr_id: f8d820e4-b4fd-4c34-a0a6-c6ec93108851
    status: accepted
    candidate_group_id: f3dc5d23-ec5a-453d-880d-3f740a5b058d
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Adoption of @/lib/utils Internal Utility Module for Presentation Components: Presentation Components Requiring Shared Utility Functions

Scope: applies to `apps/dashboard/src/components/**`.

These rules are ALWAYS ACTIVE for presentation components requiring shared utility functions.

### Rules

- **R-UTIL-001** MUST: Presentation components requiring shared utility functions MUST import them exclusively from the @/lib/utils internal module rather than declaring local helper functions or importing duplicate routines.

### Verify

```bash
# Discover and execute the repository type-checking script
npm run typecheck

# Discover and execute the repository test suite
npm run test

# Discover and execute the repository linter
npm run lint
```

**Accept when:**
- Type checking completes with zero diagnostic errors across all components importing the utility module.
- All automated unit and regression tests for utility helpers and consuming components pass without failures.
- Linting checks confirm no duplicate local utility implementations or forbidden relative paths exist.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration validation pipelines and peer code review.
</enforcement>