---
version: 2
name: cross-cutting-applications-import-primitives-through-m-ed63
description: "Adoption of @openstatus/ui Shared Component Library: Applications Import Primitives Through Modular Component"
metadata:
  actual:
    adr_id: ed6341b0-f6d1-49fb-bf32-a8c02f27f407
    status: accepted
    candidate_group_id: 5002540a-ce29-430d-b1f1-772828a27f05
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Adoption of @openstatus/ui Shared Component Library: Applications Import Primitives Through Modular Component

Scope: applies to `apps/dashboard/src/components/**`.

These rules are ALWAYS ACTIVE for user interface components, dashboard form screens, composite selection trees, and data table column views requiring standardized UI controls.

### Rules

- **R-UI-001** SHOULD: Applications SHOULD import UI primitives through modular component paths within @openstatus/ui rather than monolithic package bundles to optimize consumer dependency graphs.

### Verify

```bash
# Discover and execute the project type-checking script to verify module imports and component props
# Discover and execute the linting and structural analysis suite to ensure component imports adhere to modular path conventions
# Discover and execute the automated test runner to validate form interaction and selection state handling
```

**Accept when:**
- Static analysis and type checking pass without errors or unresolved module references across all consuming components.
- Interactive form and selection controls render properly and respond to state changes without console warnings or unhandled exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated CI pipelines, peer code review, and strict import path compliance.
</enforcement>