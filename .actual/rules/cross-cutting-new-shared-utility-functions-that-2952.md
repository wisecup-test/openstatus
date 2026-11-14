---
version: 2
name: cross-cutting-new-shared-utility-functions-that-2952
description: "Adoption of @/lib/utils Internal Utility Module for Presentation Components: New Shared Utility Functions That Operate"
metadata:
  actual:
    adr_id: 2952a14f-cfba-4346-87bf-a400706dc62a
    status: accepted
    candidate_group_id: f3dc5d23-ec5a-453d-880d-3f740a5b058d
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Adoption of @/lib/utils Internal Utility Module for Presentation Components: New Shared Utility Functions That Operate

Scope: applies to `apps/dashboard/src/components/**`.

These rules are ALWAYS ACTIVE for presentation tier components including data table cells, layout containers, and user interface elements that share utility logic across the application.

### Rules

- **R-UTIL-001** SHOULD: New shared utility functions that operate across multiple presentation components SHOULD be centralized in the @/lib/utils module with comprehensive unit test coverage.
- **R-UTIL-002** MANDATORY: Consumers MUST derive all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-UTIL-003** MANDATORY: Before writing code that uses a versioned library, developers MUST execute the lock-version grounding steps in order (find manifest, identify build tool, inspect lock/resolution artifact, look up official documentation for exact version, confirm API exists, re-run per dependency at point of use).
- **R-UTIL-004** MANDATORY: Ensure all exported functions within the internal utility module are pure functions without global state or side effects.
- **R-UTIL-005** MANDATORY: Structure utility functions with explicit TypeScript type signatures and parameter validation to prevent runtime errors in consuming components.

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
Claude Code MUST NOT skip or defer verification. Verification is enforced via automated continuous integration validation pipelines, peer code review, and pull request blocking for non-compliant changes.
</enforcement>