---
version: 2
name: cross-cutting-developers-inspect-repository-lock-artif-7a1e
description: "Adoption of @/lib/utils Internal Utility Module for Presentation Components: Developers Inspect Repository Lock Artifact Verify"
metadata:
  actual:
    adr_id: 7a1e91c4-c2c6-4457-8367-f97d2d89a46c
    status: accepted
    candidate_group_id: f3dc5d23-ec5a-453d-880d-3f740a5b058d
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Adoption of @/lib/utils Internal Utility Module for Presentation Components: Developers Inspect Repository Lock Artifact Verify

Scope: applies to `apps/dashboard/src/components/**`.

These rules are ALWAYS ACTIVE for presentation tier components including data table cells, layout containers, and user interface elements that share utility logic across the application.

### Rules

- **R-UTIL-001** MUST: Developers MUST inspect the repository lock artifact to verify the resolved version of any third-party dependency supporting utility routines before invoking or expanding module exports.

### Verify

```bash
# Discover and execute repository type-checking, test suite, and linter scripts according to project conventions
```

**Accept when:**
- Type checking completes with zero diagnostic errors across all components importing the utility module.
- All automated unit and regression tests for utility helpers and consuming components pass without failures.
- Linting checks confirm no duplicate local utility implementations or forbidden relative paths exist.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>