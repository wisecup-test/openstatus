---
version: 2
name: cross-cutting-status-block-visual-components-not-20b9
description: "Adoption of @openstatus/ui/components/blocks/status-i18n for Status Block Localization: Status Block Visual Components Not Bypass"
metadata:
  actual:
    adr_id: 20b9a780-8e97-4c8d-a2a4-d94a15a2000d
    status: accepted
    candidate_group_id: b9b70b53-2d67-492e-a095-8040301ca6c7
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/blocks/**
---

# Adoption of @openstatus/ui/components/blocks/status-i18n for Status Block Localization: Status Block Visual Components Not Bypass

Scope: applies to `packages/ui/src/components/blocks/**`.

These rules are ALWAYS ACTIVE for all status block UI components and status utility modules rendering user-facing text or formatted dates.

### Rules

- **R-STATUS-001** MUST_NOT: Status block visual components MUST NOT bypass @openstatus/ui/components/blocks/status-i18n by importing disparate external internationalization libraries directly into presentation blocks.

### Verify

```bash
# Discover and execute repository linting and static analysis scripts to verify status block component imports
# Run module dependency boundary validation checks to ensure no hardcoded string literals or unauthorized localization imports exist
```

**Accept when:**
- All status block presentation components source localized strings exclusively from @openstatus/ui/components/blocks/status-i18n
- Static analysis and verification suites pass without detecting inline hardcoded text literals or unauthorized localization imports in status block components
- Dependency boundary verification confirms zero unapproved localization dependencies imported across block components

<enforcement>
Claude Code MUST NOT skip or defer verification. All status block localization implementations must be verified via repository linting and dependency boundary checks.
</enforcement>