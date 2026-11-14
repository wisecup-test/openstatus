---
version: 2
name: cross-cutting-developers-use-class-variance-authority-5af5
description: "Adoption of class-variance-authority for Component Variant Styling: Developers Use Class Variance Authority Define"
metadata:
  actual:
    adr_id: 5af5ffcc-dc6d-46a2-b80b-84e65097ba65
    status: accepted
    candidate_group_id: e46753c3-3ce4-4422-991e-d39a1edcb3cc
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Adoption of class-variance-authority for Component Variant Styling: Developers Use Class Variance Authority Define

Scope: applies to `apps/dashboard/src/components/**`.

These rules are ALWAYS ACTIVE for presentation and UI component declarations requiring dynamic styling variants or state-based styling.

### Rules

- **R-CVA-001** MUST: Developers MUST use class-variance-authority to define component variant schemas and compute composite style classes for multi-variant UI elements.

### Verify

```bash
discover_repository_linter && execute_linter
discover_repository_typechecker && execute_typecheck
discover_repository_test_runner && execute_component_tests
```

**Accept when:**
- All component variant definitions compile without type diagnostics under the project typechecker.
- Component unit and visual regression tests pass without class resolution errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>