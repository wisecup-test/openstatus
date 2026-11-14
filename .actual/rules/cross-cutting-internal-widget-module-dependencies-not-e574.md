---
version: 2
name: cross-cutting-internal-widget-module-dependencies-not-e574
description: "Adoption of Internal Widget Module in Component Utilities: Internal Widget Module Dependencies Not Introduce"
metadata:
  actual:
    adr_id: e574ae7f-a0e4-43e5-8dc8-ee15eabfc2d7
    status: accepted
    candidate_group_id: c527c359-f916-4fe9-86b1-219e72820e03
    evidence:
      source: deep
  scope:
    root: packages/react
    paths:
      - src/**
---

# Adoption of Internal Widget Module in Component Utilities: Internal Widget Module Dependencies Not Introduce

Scope: applies to `packages/react/src/**`.

These rules are ALWAYS ACTIVE for utility modules and helper functions within component packages interacting with internal widget modules.

### Rules

- **R-WIDGET-001** MUST_NOT: Internal widget module dependencies MUST NOT introduce bidirectional or circular dependency cycles between component utilities and concrete widget definitions.

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
Claude Code MUST NOT skip or defer verification.
</enforcement>