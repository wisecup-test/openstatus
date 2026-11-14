---
version: 2
name: cross-cutting-developers-not-implement-hoc-ternary-5cc5
description: "Adoption of class-variance-authority for Component Variant Styling: Developers Not Implement Hoc Ternary Chains"
metadata:
  actual:
    adr_id: 5cc54d70-0da3-48ce-abdd-b0618805c703
    status: accepted
    candidate_group_id: e46753c3-3ce4-4422-991e-d39a1edcb3cc
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Adoption of class-variance-authority for Component Variant Styling: Developers Not Implement Hoc Ternary Chains

Scope: applies to `apps/dashboard/src/components/**`.

These rules are ALWAYS ACTIVE for presentation and UI component declarations requiring dynamic styling variants or state-based styling.

### Rules

- **R-CVA-001** MUST_NOT: Developers MUST_NOT implement ad-hoc ternary chains or unvalidated string concatenations for component styling states where a variant schema can be modeled.

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