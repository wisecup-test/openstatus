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

These rules are ALWAYS ACTIVE for all reusable user interface primitive components within the design system package requiring polymorphic element rendering.

### Rules

- **R-POL-001** MUST_NOT: Components implementing polymorphic delegation MUST NOT generate extraneous container wrapper elements in the DOM tree.

### Verify

```bash
# Discover the project package runner and execute the automated test suite across the user interface package
# Discover and run the project type checking and linting scripts to validate slot property typing and component interface contracts
package_runner=$(npm run --silent | grep -q pnpm && echo "pnpm" || (yarn --version >/dev/null 2>&1 && echo "yarn" || echo "npm"))
$package_runner test
$package_runner lint
```

**Accept when:**
- All user interface components delegating element rendering via slot primitives pass static type checking and property validation without compilation errors
- Automated component test suites confirm that slotted child elements correctly receive merged event handlers, attributes, and class names without extraneous DOM wrapper elements

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via static analysis, type checking, and automated test suites.
</enforcement>