---
version: 2
name: cross-cutting-component-variant-schemas-defined-via-874c
description: "class-variance-authority for UI Component Variants: Component Variant Schemas Defined Via Class"
metadata:
  actual:
    adr_id: 874c78d0-e929-4748-8d5e-568e284ab961
    status: accepted
    candidate_group_id: 62b6fa39-2353-401c-8aa0-215823a99517
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/ui/**
---

# class-variance-authority for UI Component Variants: Component Variant Schemas Defined Via Class

Scope: applies to `packages/ui/src/components/ui/**`.

These rules are ALWAYS ACTIVE for user interface components authoring visual variants, intents, or size modifiers within the design system package.

### Rules

- **R-CVA-001** MUST: Component variant schemas defined via class-variance-authority MUST export their derived variant property types to ensure type safety across consuming modules.

### Verify

```bash
# Discover and run the project's type-checking script from the repository manifest to verify variant prop type compliance.
# Discover and execute the project's linter and component test runners to ensure variant schemas conform to style guidelines.
```

**Accept when:**
- Type checking passes without errors across all component definitions importing class-variance-authority.
- All component test suites validate that declared variants render their corresponding styling configurations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration type-checking, automated test verification, and peer code review are mandatory for all component variant schemas.
</enforcement>