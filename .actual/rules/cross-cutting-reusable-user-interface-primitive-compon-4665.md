---
version: 2
name: cross-cutting-reusable-user-interface-primitive-compon-4665
description: "Adoption of @radix-ui/react-slot for Component Composition: Reusable User Interface Primitive Components Requiring"
metadata:
  actual:
    adr_id: 4665b69c-7796-4d41-a092-ffa73215af9e
    status: accepted
    candidate_group_id: ade8b01e-79c5-4003-b747-cd49e26b6a01
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/ui/**
---

# Adoption of @radix-ui/react-slot for Component Composition: Reusable User Interface Primitive Components Requiring

Scope: applies to `packages/ui/src/components/ui/**`.

These rules are ALWAYS ACTIVE for reusable user interface primitive components within the design system package requiring polymorphic element rendering and components supporting rendering as alternate tags or navigation elements.

### Rules

- **R-SLOT-001** MUST: Reusable user interface primitive components requiring polymorphic element rendering MUST adopt @radix-ui/react-slot and delegate element composition to slot primitives rather than implementing ad-hoc element switching.

### Verify

```bash
# Discover the project package runner and execute the automated test suite across the UI package
# Discover and run project type checking and linting scripts to validate slot property typing and component contracts
```

**Accept when:**
- All user interface components delegating element rendering via slot primitives pass static type checking and property validation without compilation errors
- Automated component test suites confirm that slotted child elements correctly receive merged event handlers, attributes, and class names without extraneous DOM wrapper elements

<enforcement>
Claude Code MUST NOT skip or defer verification. Pull request checks fail when non-standard element wrapper or dynamic tag patterns are introduced in shared components.
</enforcement>