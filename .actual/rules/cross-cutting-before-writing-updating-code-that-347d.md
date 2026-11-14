---
version: 2
name: cross-cutting-before-writing-updating-code-that-347d
description: "Adoption of @radix-ui/react-slot for Component Composition: Before Writing Updating Code That Depends"
metadata:
  actual:
    adr_id: 347d2049-819c-4789-800d-7f8e3be764a5
    status: accepted
    candidate_group_id: ade8b01e-79c5-4003-b747-cd49e26b6a01
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/ui/**
---

# Adoption of @radix-ui/react-slot for Component Composition: Before Writing Updating Code That Depends

Scope: applies to `packages/ui/src/components/ui/**`.

These rules are ALWAYS ACTIVE for reusable user interface primitive components within the design system package requiring polymorphic element rendering and components that support rendering as alternate tags or navigation elements.

### Rules

- **R-RADIX-001** MUST: Before writing or updating code that depends on @radix-ui/react-slot, the consumer MUST inspect the repository dependency manifest and lock resolution artifact to determine the exact resolved library version and verify API compatibility against authoritative reference documentation.

### Verify

```bash
# Discover the project package runner and execute the automated test suite across the user interface package
# Discover and run the project type checking and linting scripts to validate slot property typing and component interface contracts
```

**Accept when:**
- All user interface components delegating element rendering via slot primitives pass static type checking and property validation without compilation errors
- Automated component test suites confirm that slotted child elements correctly receive merged event handlers, attributes, and class names without extraneous DOM wrapper elements

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>