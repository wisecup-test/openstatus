---
version: 2
name: cross-cutting-domain-specific-iconography-representing-a0d5
description: "Adoption of lucide-react for Dashboard Iconography: Domain Specific Iconography Representing Brand Trademarks"
metadata:
  actual:
    adr_id: a0d51697-3ba9-40e3-abec-4c8f72e98610
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

# Adoption of lucide-react for Dashboard Iconography: Domain Specific Iconography Representing Brand Trademarks

Scope: applies to `apps/dashboard/src/app/**`, `apps/dashboard/src/components/**`.

These rules are ALWAYS ACTIVE for user interface components, command palettes, navigation menus, and data table interface elements within the dashboard application.

### Rules

- **R-LUCIDE-001** MAY: Domain-specific iconography representing brand trademarks or proprietary platform glyphs MAY be imported from dedicated internal icon packages when lucide-react does not supply the required asset.

### Verify

```bash
# Discover and execute project type-checking and component test scripts defined in the repository manifest
# Run project static analysis and linting suites to ensure no unapproved inline SVG assets or rogue icon libraries are imported
```

**Accept when:**
- All component views render visual icons through named imports from lucide-react without unresolved import errors.
- Project static analysis and type verification suites pass with zero errors across all affected component files.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis, lint rules, and peer code reviews enforce compliance.
</enforcement>