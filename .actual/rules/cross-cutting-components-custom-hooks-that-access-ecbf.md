---
version: 2
name: cross-cutting-components-custom-hooks-that-access-ecbf
description: "React Client Hook Interaction Pattern: Components Custom Hooks That Access Browser"
metadata:
  actual:
    adr_id: ecbfb45e-6559-4924-8f3e-b6af19e5164d
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

# React Client Hook Interaction Pattern: Components Custom Hooks That Access Browser

Scope: applies to `packages/ui/src/components/**`, `packages/ui/src/hooks/**`, `packages/ui/src/lib/**`.

These rules are ALWAYS ACTIVE for all interactive UI components, interactive blocks, and custom interaction hooks within the frontend presentation package, as well as modules requiring DOM event subscriptions, browser runtime access, animated transitions, or responsive layout tracking.

### Rules

- **R-REACT-001** MUST: Components and custom hooks that access browser runtime capabilities or attach DOM event listeners MUST encapsulate those operations inside effect hooks and provide explicit cleanup routines to release resources on teardown.

### Verify

```bash
# Discover and run the project test runner to execute unit and interaction test suites across interactive components and custom hooks.
# Discover and execute the project linter to verify compliance with React hook dependency rules and client boundary directives.
# Discover and run the project type checker to validate component properties, hook return types, and event handler contracts.
```

**Accept when:**
- All interactive UI components and custom interaction hooks pass unit and interaction test suites with clean lifecycle cleanup and zero memory leak warnings.
- Linter verification confirms compliance with React hook dependency rules and client boundary directives across all UI modules.
- Project type verification passes with zero type errors for all component properties, hook return contracts, and event callback signatures.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>