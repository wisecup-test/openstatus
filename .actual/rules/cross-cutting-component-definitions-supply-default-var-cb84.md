---
version: 2
name: cross-cutting-component-definitions-supply-default-var-cb84
description: "class-variance-authority for UI Component Variants: Component Definitions Supply Default Variant Selections"
metadata:
  actual:
    adr_id: cb844e34-1e8e-40e2-8d5e-23bdea49ff8c
    status: accepted
    candidate_group_id: 62b6fa39-2353-401c-8aa0-215823a99517
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/ui/**
---

# class-variance-authority for UI Component Variants: Component Definitions Supply Default Variant Selections

Scope: applies to `packages/ui/src/components/ui/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CVA-001** SHOULD: Component definitions SHOULD supply default variant selections within the class-variance-authority configuration to ensure consistent baseline rendering.
- **R-CVA-002** MANDATORY: When authoring a component variant schema, define the baseline styles first, followed by specific variant keys such as intent and size, and finally any compound variant overrides.
- **R-CVA-003** MANDATORY: Export the derived variant property type alongside the component declaration so consumers can type-check variant props without manual interface re-declaration.

### Verify

```bash
# Discover and run the project's type-checking script from the repository manifest to verify variant prop type compliance.
# Discover and execute the project's linter and component test runners to ensure variant schemas conform to style guidelines.
```

**Accept when:**
- Type checking passes without errors across all component definitions importing class-variance-authority.
- All component test suites validate that declared variants render their corresponding styling configurations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration type-checking, automated test verification, and peer code review enforce these rules; PRs with untyped or ad-hoc conditional component styling variants are blocked.
</enforcement>