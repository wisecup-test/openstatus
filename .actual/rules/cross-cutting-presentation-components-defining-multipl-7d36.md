---
version: 2
name: cross-cutting-presentation-components-defining-multipl-7d36
description: "class-variance-authority for UI Component Variants: Presentation Components Defining Multiple Visual Styles"
metadata:
  actual:
    adr_id: 7d3607fe-2ae9-43f4-92e8-85ce09458954
    status: accepted
    candidate_group_id: 62b6fa39-2353-401c-8aa0-215823a99517
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/ui/**
---

# class-variance-authority for UI Component Variants: Presentation Components Defining Multiple Visual Styles

Scope: applies to `packages/ui/src/components/ui/**`.

These rules are ALWAYS ACTIVE for user interface components authoring visual variants, intents, or size modifiers within the design system package.

### Rules

- **R-CVA-001** MUST: All presentation components defining multiple visual styles or size configurations MUST declare their variant schemas using class-variance-authority.

### Verify

```bash
# Discover and run the project's type-checking script from the repository manifest to verify variant prop type compliance.
# Discover and execute the project's linter and component test runners to ensure variant schemas conform to style guidelines.
```

**Accept when:**
- Type checking passes without errors across all component definitions importing class-variance-authority.
- All component test suites validate that declared variants render their corresponding styling configurations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration type-checking, automated test verification, and peer code review enforce these rules; violations will block pull requests.
</enforcement>