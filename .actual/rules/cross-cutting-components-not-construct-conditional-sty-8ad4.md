---
version: 2
name: cross-cutting-components-not-construct-conditional-sty-8ad4
description: "class-variance-authority for UI Component Variants: Components Not Construct Conditional Styling Variants"
metadata:
  actual:
    adr_id: 8ad40de8-3d0e-4022-bcf4-80ec566ffe94
    status: accepted
    candidate_group_id: 62b6fa39-2353-401c-8aa0-215823a99517
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/ui/**
---

# class-variance-authority for UI Component Variants: Components Not Construct Conditional Styling Variants

Scope: applies to `packages/ui/src/components/ui/**`.

These rules are ALWAYS ACTIVE for presentation components authoring visual variants, intents, or size modifiers within the design system package.

### Rules

- **R-CVA-001** MUST_NOT: Components MUST NOT construct conditional styling variants via manual string concatenation or uncoordinated boolean flag branching.

### Verify

```bash
# Discover and run the project's type-checking script from the repository manifest to verify variant prop type compliance.
# Discover and execute the project's linter and component test runners to ensure variant schemas conform to style guidelines.
```

**Accept when:**
- Type checking passes without errors across all component definitions importing class-variance-authority.
- All component test suites validate that declared variants render their corresponding styling configurations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via continuous integration type-checking, automated test verification, and peer code review.
</enforcement>