---
version: 2
name: cross-cutting-components-requiring-compound-styling-pe-72f7
description: "class-variance-authority for UI Component Variants: Components Requiring Compound Styling Permutations Declare"
metadata:
  actual:
    adr_id: 72f7315f-5881-4794-86fd-db3aa3c38e19
    status: accepted
    candidate_group_id: 62b6fa39-2353-401c-8aa0-215823a99517
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/ui/**
---

# class-variance-authority for UI Component Variants: Components Requiring Compound Styling Permutations Declare

Scope: applies to `packages/ui/src/components/ui/**`.

These rules are ALWAYS ACTIVE for all presentation components authoring visual variants, intents, or size modifiers within the design system package.

### Rules

- **R-CVA-001** MAY: Components requiring compound styling permutations MAY declare compound variants within the class-variance-authority schema to coordinate interdependent modifier states.

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