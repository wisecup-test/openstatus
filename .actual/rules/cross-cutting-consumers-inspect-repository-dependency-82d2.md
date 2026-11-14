---
version: 2
name: cross-cutting-consumers-inspect-repository-dependency-82d2
description: "React Client Hook Interaction Pattern: Consumers Inspect Repository Dependency Manifest Authoritative"
metadata:
  actual:
    adr_id: 82d2acee-5f59-48a0-864b-b61abd3138dc
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

# React Client Hook Interaction Pattern: Consumers Inspect Repository Dependency Manifest Authoritative

Scope: applies to `packages/ui/src/components/**`, `packages/ui/src/hooks/**`, `packages/ui/src/lib/**`.

These rules are ALWAYS ACTIVE for all interactive UI components, interactive blocks, and custom interaction hooks within the frontend presentation package, and modules requiring DOM event subscriptions, browser runtime access, animated transitions, or responsive layout tracking.

### Rules

- **R-REACT-001** MUST: Consumers MUST inspect the repository dependency manifest and authoritative lock artifact to verify the exact resolved version of the React library before implementing or updating client interaction hooks.

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