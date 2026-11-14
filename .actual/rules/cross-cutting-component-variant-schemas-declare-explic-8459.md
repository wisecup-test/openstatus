---
version: 2
name: cross-cutting-component-variant-schemas-declare-explic-8459
description: "Adoption of class-variance-authority for Component Variant Styling: Component Variant Schemas Declare Explicit Default"
metadata:
  actual:
    adr_id: 84594ab4-3c93-4efe-a64e-6ce893e54273
    status: accepted
    candidate_group_id: a937ff75-96f0-4a05-a5bd-101569899b32
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/components/**
---

# Adoption of class-variance-authority for Component Variant Styling: Component Variant Schemas Declare Explicit Default

Scope: applies to `apps/status-page/src/components/**`.

These rules are ALWAYS ACTIVE for all presentation components that expose configurable visual variants, intents, or sizes, and shared design system primitives and form elements requiring multi-state styling contracts.

### Rules

- **R-CVA-001** SHOULD: Component variant schemas SHOULD declare explicit default variants for all defined variant dimensions.

### Verify

```bash
# Discover and execute the repository test runner script from the project manifest
# Discover and execute the static analysis script from the project manifest
```

**Accept when:**
- All component test suites pass verifying correct styling application across all declared variant combinations.
- Static analysis and type checking complete with zero errors related to variant schema configurations.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>