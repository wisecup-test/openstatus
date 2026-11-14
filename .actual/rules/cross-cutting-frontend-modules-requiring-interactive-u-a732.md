---
version: 2
name: cross-cutting-frontend-modules-requiring-interactive-u-a732
description: "Adoption of @openstatus/ui Shared Component Library: Frontend Modules Requiring Interactive User Interface"
metadata:
  actual:
    adr_id: a7326c09-06bb-4f16-b6df-e76a6b293567
    status: accepted
    candidate_group_id: 5002540a-ce29-430d-b1f1-772828a27f05
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Adoption of @openstatus/ui Shared Component Library: Frontend Modules Requiring Interactive User Interface

Scope: applies to `apps/dashboard/src/components/**`.

These rules are ALWAYS ACTIVE for frontend modules, dashboard form screens, composite selection trees, and data table column views requiring standardized UI controls.

### Rules

- **R-UI-001** MUST: Frontend modules requiring interactive user interface controls MUST import shared design components directly from the internal @openstatus/ui module.

### Verify

```bash
# Discover and execute the repository type-checking script to verify module imports and component props
# Discover and execute the project linting and structural analysis suite to ensure component imports adhere to modular path conventions
# Discover and execute the automated test runner to validate form interaction and selection state handling across consuming views
```

**Accept when:**
- Static analysis and type checking pass without errors or unresolved module references across all consuming components.
- Interactive form and selection controls render properly and respond to state changes without console warnings or unhandled exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated continuous integration pipelines, peer code review, and automated build checks.
</enforcement>