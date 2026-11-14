---
version: 2
name: cross-cutting-slotted-child-elements-passed-polymorphi-259b
description: "Adoption of @radix-ui/react-slot for Component Composition: Slotted Child Elements Passed Polymorphic Components"
metadata:
  actual:
    adr_id: 259bb699-f8b6-4e40-8d8f-78f597248ddd
    status: accepted
    candidate_group_id: ade8b01e-79c5-4003-b747-cd49e26b6a01
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/ui/**
---

# Adoption of @radix-ui/react-slot for Component Composition: Slotted Child Elements Passed Polymorphic Components

Scope: applies to `packages/ui/src/components/ui/**`.

These rules are ALWAYS ACTIVE for reusable user interface primitive components within the design system package requiring polymorphic element rendering.

### Rules

- **R-SLOT-001** MUST: Slotted child elements passed to polymorphic components MUST support property forwarding and reference forwarding to accept merged event handlers and attributes.

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