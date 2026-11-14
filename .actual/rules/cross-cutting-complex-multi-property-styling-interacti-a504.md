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
- **R-CVA-002** MANDATORY: Construct component variant schemas outside the render lifecycle to prevent redundant schema initialization.
- **R-CVA-003** MANDATORY: Compose variant schema output with shared class-merging utilities to allow safe downstream class overrides.
- **R-CVA-004** MANDATORY (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, identify the dependency manifest, build tool, and exact resolved version from the repository lock or resolution artifact. Look up official documentation for that exact version and confirm every API call exists before using it.

### Verify

```bash
# Discover and run the component test suite and static analysis script from the project manifest
# Example workflow:
# npm test
# npm run lint
```

**Accept when:**
- All component test suites pass verifying correct styling application across all declared variant combinations.
- Static analysis and type checking complete with zero errors related to variant schema configurations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Pull requests introducing manual conditional class concatenation for multi-variant components will be blocked.
</enforcement>