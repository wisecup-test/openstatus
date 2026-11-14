---
version: 2
name: cross-cutting-components-hooks-not-access-browser-6bdc
description: "React Client Hook Interaction Pattern: Components Hooks Not Access Browser Specific"
metadata:
  actual:
    adr_id: 6bdc1aeb-b1c4-4ae7-9efc-a1ee412bfce6
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

# React Client Hook Interaction Pattern: Components Hooks Not Access Browser Specific

Scope: applies to `packages/ui/src/components/**`, `packages/ui/src/hooks/**`, `packages/ui/src/lib/**`.

These rules are ALWAYS ACTIVE for all interactive UI components, interactive blocks, and custom interaction hooks within the frontend presentation package, including modules requiring DOM event subscriptions, browser runtime access, animated transitions, or responsive layout tracking.

### Rules

- **R-REACT-001** MUST_NOT: Components and hooks MUST_NOT access browser-specific global objects or execute DOM mutations during render execution prior to mount lifecycle completion.

### Verify

```bash
# Discover and run the project test runner to execute unit and interaction test suites
# Discover and execute the project linter to verify compliance with React hook dependency rules and client boundary directives
# Discover and run the project type checker to validate component properties, hook return types, and event handler contracts
```

**Accept when:**
- All interactive UI components and custom interaction hooks pass unit and interaction test suites with clean lifecycle cleanup and zero memory leak warnings.
- Linter verification confirms compliance with React hook dependency rules and client boundary directives across all UI modules.
- Project type verification passes with zero type errors for all component properties, hook return contracts, and event callback signatures.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests containing hook dependency omissions or unencapsulated browser API access are blocked from merging.
</enforcement>