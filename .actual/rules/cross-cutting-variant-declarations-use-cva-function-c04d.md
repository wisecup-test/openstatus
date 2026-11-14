---
version: 2
name: cross-cutting-variant-declarations-use-cva-function-c04d
description: "Adoption of class-variance-authority for Component Variant Styling: Variant Declarations Use Cva Function Declare"
metadata:
  actual:
    adr_id: c04dcfd4-1bd8-4e05-b40f-20941a29788e
    status: accepted
    candidate_group_id: e46753c3-3ce4-4422-991e-d39a1edcb3cc
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Adoption of class-variance-authority for Component Variant Styling: Variant Declarations Use Cva Function Declare

Scope: applies to `apps/dashboard/src/components/**`.

These rules are ALWAYS ACTIVE for presentation and UI component declarations requiring dynamic styling variants or state-based styling.

### Rules

- **R-CVA-001** MUST: Variant declarations MUST use the cva function to declare base class names and variant mapping tables with explicit default variants.

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