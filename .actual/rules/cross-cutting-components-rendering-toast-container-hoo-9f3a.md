---
version: 2
name: cross-cutting-components-rendering-toast-container-hoo-9f3a
description: "Adoption of sonner for Toast Notification Management: Components Rendering Toast Container Hooks Triggering"
metadata:
  actual:
    adr_id: 9f3afe7b-1fe0-44af-8d0b-e7223f46f94a
    status: accepted
    candidate_group_id: 9abf003d-098e-4232-bd99-6521c772f65f
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/ui/**
      - src/hooks/**
---

# Adoption of sonner for Toast Notification Management: Components Rendering Toast Container Hooks Triggering

Scope: applies to `packages/ui/src/components/ui/**`, `packages/ui/src/hooks/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SON-001** MUST: Components rendering the toast container and hooks triggering toast feedback MUST operate within client-rendered boundaries marked with the client component directive.

### Verify

```bash
# Discover and run the workspace test suite to validate toast notification rendering and interaction hook behavior.
# Discover and run the workspace static analysis and linting checks to ensure compliance with client boundary directives.
```

**Accept when:**
- All toast notification components render without runtime errors under client execution boundaries.
- Interactive hooks successfully dispatch toast notifications using sonner API bindings without regressions.
- Static analysis and test suites pass cleanly across all UI package modules.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>