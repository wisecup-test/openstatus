---
version: 2
name: cross-cutting-components-requiring-variant-based-styli-08f4
description: "Adoption of class-variance-authority for Component Variant Styling: Components Requiring Variant Based Styling Use"
metadata:
  actual:
    adr_id: 08f4f8a8-94c0-4920-9387-cf8c3877c371
    status: accepted
    candidate_group_id: a937ff75-96f0-4a05-a5bd-101569899b32
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/components/**
---

# Adoption of class-variance-authority for Component Variant Styling: Components Requiring Variant Based Styling Use

Scope: applies to `apps/status-page/src/components/**`.

These rules are ALWAYS ACTIVE for all presentation components that expose configurable visual variants, intents, or sizes, and shared design system primitives and form elements requiring multi-state styling contracts.

### Rules

- **R-CVA-001** MUST: Components requiring variant-based styling MUST use class-variance-authority to declare variant configurations and generate styling class permutations.

### Verify

```bash
# Discover the repository test runner script from the project manifest and execute the component test suite to validate variant rendering.
# Discover the static analysis script from the project manifest and run it to verify that component styling conforms to standard variant declarations without unmanaged class string concatenation.
```

**Accept when:**
- All component test suites pass verifying correct styling application across all declared variant combinations.
- Static analysis and type checking complete with zero errors related to variant schema configurations.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>