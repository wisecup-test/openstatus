---
version: 2
name: cross-cutting-event-callback-functions-passed-child-706b
description: "React Client Hook Interaction Pattern: Event Callback Functions Passed Child Component"
metadata:
  actual:
    adr_id: 706b6c87-c294-4e13-aa5c-2b9b07ab0781
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

# React Client Hook Interaction Pattern: Event Callback Functions Passed Child Component

Scope: applies to `packages/ui/src/components/**`, `packages/ui/src/hooks/**`, `packages/ui/src/lib/**`.

These rules are ALWAYS ACTIVE for all interactive UI components, interactive blocks, and custom interaction hooks within the frontend presentation package, including modules requiring DOM event subscriptions, browser runtime access, animated transitions, or responsive layout tracking.

### Rules

- **R-REACT-001** SHOULD: Event callback functions passed to child component hierarchies SHOULD be stabilized using memoized callback hooks to minimize unnecessary downstream renders.

### Verify

```bash
# Discover and execute the project test runner to execute unit and interaction test suites across interactive components and custom hooks.
# Discover and execute the project linter to verify compliance with React hook dependency rules and client boundary directives.
# Discover and run the project type checker to validate component properties, hook return types, and event handler contracts.
```

**Accept when:**
- All interactive UI components and custom interaction hooks pass unit and interaction test suites with clean lifecycle cleanup and zero memory leak warnings.
- Linter verification confirms compliance with React hook dependency rules and client boundary directives across all UI modules.
- Project type verification passes with zero type errors for all component properties, hook return contracts, and event callback signatures.

<enforcement>
Claude Code MUST NOT skip or defer verification. All changes must be verified through the project test runner, linter, and type checker.
</enforcement>