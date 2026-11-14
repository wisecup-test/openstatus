---
version: 2
name: cross-cutting-composite-presentation-components-wrap-o-4572
description: "Adoption of @openstatus/ui Shared Component Library: Composite Presentation Components Wrap Openstatus Primitives"
metadata:
  actual:
    adr_id: 45723aa2-f140-467f-87cf-186abd394361
    status: accepted
    candidate_group_id: 5002540a-ce29-430d-b1f1-772828a27f05
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Adoption of @openstatus/ui Shared Component Library: Composite Presentation Components Wrap Openstatus Primitives

Scope: applies to `apps/dashboard/src/components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OPEN-001** MAY: Composite presentation components MAY wrap @openstatus/ui primitives to encapsulate specialized hierarchical or multi-selection behaviors.

### Verify

```bash
# Discover and execute repository type-checking, linting, and automated test commands
# (Commands are dynamically discovered from the project repository configuration)
```

**Accept when:**
- Static analysis and type checking pass without errors or unresolved module references across all consuming components.
- Interactive form and selection controls render properly and respond to state changes without console warnings or unhandled exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>