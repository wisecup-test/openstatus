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

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-UI-001** MUST: Reusable user interface primitive components requiring polymorphic element rendering MUST adopt @radix-ui/react-slot and delegate element composition to slot primitives rather than implementing ad-hoc element switching.

### Verify

```bash
# Discover the project package runner and execute the automated test suite across the user interface package
# Discover and run the project type checking and linting scripts
package_runner=$(npm run --silent 2>/dev/null | grep -q pnpm && echo "pnpm" || echo "npm")
if [ -f "pnpm-lock.yaml" ]; then
  pnpm test
  pnpm typecheck
  pnpm lint
elif [ -f "yarn.lock" ]; then
  yarn test
  yarn typecheck
  yarn lint
else
  npm test
  npm run typecheck
  npm run lint
fi
```

**Accept when:**
- All user interface components delegating element rendering via slot primitives pass static type checking and property validation without compilation errors
- Automated component test suites confirm that slotted child elements correctly receive merged event handlers, attributes, and class names without extraneous DOM wrapper elements

<enforcement>
Claude Code MUST NOT skip or defer verification. Compliance is enforced via automated CI pipeline static analysis, type checking, and peer code reviews.
</enforcement>