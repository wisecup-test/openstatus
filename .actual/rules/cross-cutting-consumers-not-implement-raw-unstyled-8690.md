---
version: 2
name: cross-cutting-consumers-not-implement-raw-unstyled-8690
description: "Adoption of @openstatus/ui Shared Component Library: Consumers Not Implement Raw Unstyled Input"
metadata:
  actual:
    adr_id: 8690841a-4b4e-44a8-896b-a00f6d857a1d
    status: accepted
    candidate_group_id: 5002540a-ce29-430d-b1f1-772828a27f05
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Adoption of @openstatus/ui Shared Component Library: Consumers Not Implement Raw Unstyled Input

Scope: applies to `apps/dashboard/src/components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-UI-001** MUST_NOT: Consumers MUST_NOT implement raw unstyled input primitives or duplicate selection control components when matching primitives exist within @openstatus/ui.

### Verify

```bash
# Discover and execute the repository type-checking script to verify module imports and component props
# Discover and execute the project linting and structural analysis suite
# Discover and execute the automated test runner
```

**Accept when:**
- Static analysis and type checking pass without errors or unresolved module references across all consuming components.
- Interactive form and selection controls render properly and respond to state changes without console warnings or unhandled exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>