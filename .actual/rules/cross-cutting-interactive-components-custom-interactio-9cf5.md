---
version: 2
name: cross-cutting-interactive-components-custom-interactio-9cf5
description: "React Client Hook Interaction Pattern: Interactive Components Custom Interaction Hooks Declare"
metadata:
  actual:
    adr_id: 9cf51333-f071-4d49-b752-4027305aff01
    status: accepted
    candidate_group_id: b1157a91-94b5-4a82-91f1-2f5f2caddd83
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/**
      - src/hooks/**
      - src/lib/**
---

# React Client Hook Interaction Pattern: Interactive Components Custom Interaction Hooks Declare

Scope: applies to `packages/ui/src/components/**`, `packages/ui/src/hooks/**`, `packages/ui/src/lib/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-REACT-001** MUST: Interactive UI components and custom interaction hooks MUST declare the client component rendering directive and manage local state, effects, and memoized values through standard React hook primitives.

### Verify

```bash
# Discover and run test runner for unit and interaction test suites
# Discover and execute project linter to verify React hook dependency rules and client boundary directives
# Discover and run project type checker to validate component properties, hook return types, and event handler contracts
```

**Accept when:**
- All interactive UI components and custom interaction hooks pass unit and interaction test suites with clean lifecycle cleanup and zero memory leak warnings.
- Linter verification confirms compliance with React hook dependency rules and client boundary directives across all UI modules.
- Project type verification passes with zero type errors for all component properties, hook return contracts, and event callback signatures.

<enforcement>
Claude Code MUST NOT skip or defer verification. All interactive UI components and custom interaction hooks must adhere to client boundary directives, hook lifecycle standards, and exhaustive dependency requirements.
</enforcement>