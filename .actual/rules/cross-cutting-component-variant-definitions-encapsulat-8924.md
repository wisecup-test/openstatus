---
version: 2
name: cross-cutting-component-variant-definitions-encapsulat-8924
description: "Adoption of class-variance-authority for Component Variant Styling: Component Variant Definitions Encapsulated Designated Schemas"
metadata:
  actual:
    adr_id: 8924e0cc-2633-42cd-9fee-174308ce21ff
    status: accepted
    candidate_group_id: a937ff75-96f0-4a05-a5bd-101569899b32
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/components/**
---

# Adoption of class-variance-authority for Component Variant Styling: Component Variant Definitions Encapsulated Designated Schemas

Scope: applies to `apps/status-page/src/components/**`.

These rules are ALWAYS ACTIVE for all presentation components that expose configurable visual variants, intents, or sizes, and shared design system primitives and form elements requiring multi-state styling contracts.

### Rules

- **R-CVA-001** MUST: Component variant definitions MUST be encapsulated in designated variant schemas rather than constructed through inline conditional string concatenation.
- **R-CVA-002** MUST: Execute lock-version grounding prior to writing code that uses a versioned library: find dependency manifest, identify build tool, inspect repository lock or resolution artifact for the exact resolved version, look up official documentation for that exact version, confirm every API exists in that version, and re-verify per dependency at point of use.
- **R-CVA-003** MUST: Construct component variant schemas outside the render lifecycle to prevent redundant schema initialization.
- **R-CVA-004** MUST: Compose variant schema output with shared class-merging utilities to allow safe downstream class overrides.

### Verify

```bash
# Discover the repository test runner script from the project manifest and execute the component test suite
# Discover the static analysis script from the project manifest and run it
```

**Accept when:**
- All component test suites pass verifying correct styling application across all declared variant combinations.
- Static analysis and type checking complete with zero errors related to variant schema configurations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>