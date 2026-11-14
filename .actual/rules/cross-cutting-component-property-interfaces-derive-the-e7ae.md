---
version: 2
name: cross-cutting-component-property-interfaces-derive-the-e7ae
description: "Adoption of class-variance-authority for Component Variant Styling: Component Property Interfaces Derive Their Variant"
metadata:
  actual:
    adr_id: e7ae2729-3871-4754-ac89-e216706dd1c5
    status: accepted
    candidate_group_id: e46753c3-3ce4-4422-991e-d39a1edcb3cc
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Adoption of class-variance-authority for Component Variant Styling: Component Property Interfaces Derive Their Variant

Scope: applies to `apps/dashboard/src/components/**`.

These rules are ALWAYS ACTIVE for presentation and UI component declarations requiring dynamic styling variants or state-based styling.

### Rules

- **R-CVA-001** SHOULD: Component property interfaces SHOULD derive their variant prop types directly from the cva declaration using the VariantProps utility type.

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