---
version: 2
name: cross-cutting-complex-components-containing-multiple-i-cc5a
description: "Adoption of @radix-ui/react-slot for Component Composition: Complex Components Containing Multiple Internal Elements"
metadata:
  actual:
    adr_id: cc5ac848-4a07-4b85-a6b9-74eabef1bd4d
    status: accepted
    candidate_group_id: ade8b01e-79c5-4003-b747-cd49e26b6a01
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/ui/**
---

# Adoption of @radix-ui/react-slot for Component Composition: Complex Components Containing Multiple Internal Elements

Scope: applies to `packages/ui/src/components/ui/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-RADIX-001** SHOULD: Complex components containing multiple internal elements SHOULD use the Slottable primitive from @radix-ui/react-slot to designate which child receives delegated properties.

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