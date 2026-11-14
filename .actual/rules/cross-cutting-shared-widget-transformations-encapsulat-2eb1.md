---
version: 2
name: cross-cutting-shared-widget-transformations-encapsulat-2eb1
description: "Adoption of Internal Widget Module in Component Utilities: Shared Widget Transformations Encapsulated Within Dedicated"
metadata:
  actual:
    adr_id: 2eb14bc3-d126-4ea0-9569-2d76f2666210
    status: accepted
    candidate_group_id: c527c359-f916-4fe9-86b1-219e72820e03
    evidence:
      source: deep
  scope:
    root: packages/react
    paths:
      - src/**
---

# Adoption of Internal Widget Module in Component Utilities: Shared Widget Transformations Encapsulated Within Dedicated

Scope: applies to `packages/react/src/**`.

These rules are ALWAYS ACTIVE for utility modules and helper functions within component packages interacting with internal widget modules.

### Rules

- **R-WIDGET-001** SHOULD: Shared widget transformations SHOULD be encapsulated within dedicated domain adapters instead of generic utility modules.
- **R-WIDGET-002** MAY: A widget utility function requires low-level internal access for backward compatibility or performance-critical serialization, subject to exception EXC-20-001.

### Verify

```bash
# Discover and run the project static analysis and linting scripts to verify module import boundaries
# Execute the repository test suite to confirm that utility functions decouple cleanly from internal widget logic
# Inspect dependency graph validation commands configured in the repository workflow to verify absence of circular dependencies
```

**Accept when:**
- All project linting and module boundary checks pass without cyclic or unauthorized relative import violations.
- Utility functions execute all unit and integration test suites successfully when isolated from widget internals.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration pipeline static analysis checks and mandatory peer code review must verify all changes.
</enforcement>