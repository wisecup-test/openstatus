---
version: 2
name: cross-cutting-complex-multi-property-styling-interacti-a504
description: "Adoption of class-variance-authority for Component Variant Styling: Complex Multi Property Styling Interactions Modeled"
metadata:
  actual:
    adr_id: a5046bf4-3ebe-4f1f-ae5d-ff76e7dfcd8b
    status: accepted
    candidate_group_id: a937ff75-96f0-4a05-a5bd-101569899b32
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/components/**
---

# Adoption of class-variance-authority for Component Variant Styling: Complex Multi Property Styling Interactions Modeled

Scope: applies to `apps/status-page/src/components/**`.

These rules are ALWAYS ACTIVE for all presentation components that expose configurable visual variants, intents, or sizes, and shared design system primitives and form elements requiring multi-state styling contracts.

### Rules

- **R-CVA-001** SHOULD: Complex multi-property styling interactions SHOULD be modeled using compound variants within the variant schema rather than nested conditional logic inside component render functions.

### Verify

```bash
# Discover and execute the test runner script from the project manifest
# Discover and execute the static analysis script from the project manifest
```

**Accept when:**
- All component test suites pass verifying correct styling application across all declared variant combinations.
- Static analysis and type checking complete with zero errors related to variant schema configurations.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>