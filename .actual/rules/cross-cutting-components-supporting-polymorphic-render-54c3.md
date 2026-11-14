---
version: 2
name: cross-cutting-components-supporting-polymorphic-render-54c3
description: "Adoption of @radix-ui/react-slot for Component Composition: Components Supporting Polymorphic Rendering Declare Aschild"
metadata:
  actual:
    adr_id: 54c37d76-bd2a-46c4-a9b5-37da88f0e678
    status: accepted
    candidate_group_id: ade8b01e-79c5-4003-b747-cd49e26b6a01
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/ui/**
---

# Adoption of @radix-ui/react-slot for Component Composition: Components Supporting Polymorphic Rendering Declare Aschild

Scope: applies to `packages/ui/src/components/ui/**`.

These rules are ALWAYS ACTIVE for reusable user interface primitive components within the design system package requiring polymorphic element rendering and components supporting rendering as alternate tags or navigation elements.

### Rules

- **R-COMP-001** MUST: Components supporting polymorphic rendering MUST declare an asChild boolean property on their interface and conditionally render the Slot primitive when the property is true.

### Verify

```bash
# Discover the project package runner and execute the automated test suite across the user interface package to verify component rendering and slot delegation.
# Discover and run the project type checking and linting scripts to validate slot property typing and component interface contracts.
```

**Accept when:**
- All user interface components delegating element rendering via slot primitives pass static type checking and property validation without compilation errors
- Automated component test suites confirm that slotted child elements correctly receive merged event handlers, attributes, and class names without extraneous DOM wrapper elements

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>