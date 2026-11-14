---
version: 2
name: cross-cutting-before-implementing-updating-component-v-14fe
description: "class-variance-authority for UI Component Variants: Before Implementing Updating Component Variant Definitions"
metadata:
  actual:
    adr_id: 14fe8f0d-7a49-407d-b7eb-5c2da5530ed0
    status: accepted
    candidate_group_id: 62b6fa39-2353-401c-8aa0-215823a99517
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/ui/**
---

# class-variance-authority for UI Component Variants: Before Implementing Updating Component Variant Definitions

Scope: applies to `packages/ui/src/components/ui/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CVA-001** MUST: Before implementing or updating component variant definitions, developers MUST inspect the repository lock or resolution artifact to determine the exact resolved version of class-variance-authority and confirm API compatibility against official version-specific documentation.

### Verify

\`\`\`bash
# Discover and run the project's type-checking script from the repository manifest
# Discover and execute the project's linter and component test runners
# Example typical discovery commands:
npm run typecheck || pnpm typecheck || yarn typecheck
npm run test || pnpm test || yarn test
\`\`\`

**Accept when:**
- Type checking passes without errors across all component definitions importing class-variance-authority.
- All component test suites validate that declared variants render their corresponding styling configurations.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>