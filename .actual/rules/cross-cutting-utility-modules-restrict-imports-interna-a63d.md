---
version: 2
name: cross-cutting-utility-modules-restrict-imports-interna-a63d
description: "Adoption of Internal Widget Module in Component Utilities: Utility Modules Restrict Imports Internal Widget"
metadata:
  actual:
    adr_id: a63d29f2-9176-4197-b47a-b0430b4430f3
    status: accepted
    candidate_group_id: c527c359-f916-4fe9-86b1-219e72820e03
    evidence:
      source: deep
  scope:
    root: packages/react
    paths:
      - src/**
---

# Adoption of Internal Widget Module in Component Utilities: Utility Modules Restrict Imports Internal Widget

Scope: applies to `packages/react/src/**`.

These rules are ALWAYS ACTIVE for utility modules and helper functions within component packages interacting with internal widget modules.

### Rules

- **R-UTIL-001** MUST: Utility modules MUST restrict imports of internal widget modules to declared public module interfaces rather than accessing unencapsulated internal submodules.

### Verify

```bash
# Discover and run the project static analysis and linting scripts to verify module import boundaries.
# Execute the repository test suite to confirm that utility functions decouple cleanly from internal widget logic.
# Inspect dependency graph validation commands configured in the repository workflow to verify absence of circular dependencies.
```

**Accept when:**
- All project linting and module boundary checks pass without cyclic or unauthorized relative import violations.
- Utility functions execute all unit and integration test suites successfully when isolated from widget internals.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests introducing unapproved internal module couplings or circular references are blocked and must be remediated.
</enforcement>