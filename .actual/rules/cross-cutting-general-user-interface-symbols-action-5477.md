---
version: 2
name: cross-cutting-general-user-interface-symbols-action-5477
description: "Adoption of @openstatus/icons and Brand Subpath for Centralized Iconography: General User Interface Symbols Action Controls"
metadata:
  actual:
    adr_id: 54777c1a-78ac-4f4c-a164-a85c201a3921
    status: accepted
    candidate_group_id: 6657c8b1-ac0f-40f5-afa9-ba814c964fbd
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/app/login/**
      - src/components/**
      - src/config/**
      - src/data/**
---

# Adoption of @openstatus/icons and Brand Subpath for Centralized Iconography: General User Interface Symbols Action Controls

Scope: applies to `apps/dashboard/src/app/login/**`, `apps/dashboard/src/components/**`, `apps/dashboard/src/config/**`, `apps/dashboard/src/data/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ICON-001** MUST: General user interface symbols, action controls, navigation markers, and status indicators MUST be imported from @openstatus/icons.

### Verify

```bash
# Discover and execute the repository static analysis and type checking suite
# Discover and execute repository linting rules
# Discover and run the project build scripts
```

**Accept when:**
- All graphical icons and provider brand marks are imported exclusively through @openstatus/icons or @openstatus/icons/brand.
- Static analysis and type verification confirm clean symbol resolution across all user interface modules.
- Build artifact inspection verifies that icon subpaths bundle cleanly without duplicate vector definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>