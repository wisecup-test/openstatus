---
version: 2
name: cross-cutting-icon-components-lucide-react-receive-4e36
description: "Adoption of lucide-react for Dashboard Iconography: Icon Components Lucide React Receive Standardized"
metadata:
  actual:
    adr_id: 4e365495-499b-46f7-9add-97806eb2e2ff
    status: accepted
    candidate_group_id: 5f56dc68-a638-4a7f-8036-e481d4a9289d
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/app/**
      - src/components/**
---

# Adoption of lucide-react for Dashboard Iconography: Icon Components Lucide React Receive Standardized

Scope: applies to `apps/dashboard/src/app/**`, `apps/dashboard/src/components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-LUCIDE-001** SHOULD: Icon components from lucide-react SHOULD receive standardized dimensional and styling properties through component class attributes rather than hardcoded inline style overrides.

### Verify

```bash
# Discover and execute the project type-checking and component test scripts defined in the repository manifest.
# Run the project static analysis and linting suites to ensure no unapproved inline SVG assets or rogue icon libraries are imported.
```

**Accept when:**
- All component views render visual icons through named imports from lucide-react without unresolved import errors.
- Project static analysis and type verification suites pass with zero errors across all affected component files.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>