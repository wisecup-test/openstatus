---
version: 2
name: cross-cutting-components-implementing-polymorphic-dele-8d85
description: "Adoption of @radix-ui/react-slot for Component Composition: Components Implementing Polymorphic Delegation Not Generate"
metadata:
  actual:
    adr_id: 8d8520da-b820-4ab4-895a-a458f2ce2fb9
    status: accepted
    candidate_group_id: ade8b01e-79c5-4003-b747-cd49e26b6a01
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/ui/**
---

# Adoption of @radix-ui/react-slot for Component Composition: Components Implementing Polymorphic Delegation Not Generate

Scope: applies to `packages/ui/src/components/ui/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-COMP-001** MUST_NOT: Components implementing polymorphic delegation MUST NOT generate extraneous container wrapper elements in the DOM tree.

### Verify

```bash
# Discover project package runner and run automated test suite across the UI package
# Discover and run project type checking and linting scripts
```

**Accept when:**
- All user interface components delegating element rendering via slot primitives pass static type checking and property validation without compilation errors
- Automated component test suites confirm that slotted child elements correctly receive merged event handlers, attributes, and class names without extraneous DOM wrapper elements

<enforcement>
Claude Code MUST NOT skip or defer verification. Compliance is verified via static analysis, type checking, and peer code reviews in the CI pipeline.
</enforcement>