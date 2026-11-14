---
version: 2
name: cross-cutting-status-block-presentation-components-rou-cdb8
description: "Adoption of @openstatus/ui/components/blocks/status-i18n for Status Block Localization: Status Block Presentation Components Route Internationalization"
metadata:
  actual:
    adr_id: cdb87ebe-6d47-4678-9ed1-ab2d2a60f3f4
    status: accepted
    candidate_group_id: b9b70b53-2d67-492e-a095-8040301ca6c7
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/blocks/**
---

# Adoption of @openstatus/ui/components/blocks/status-i18n for Status Block Localization: Status Block Presentation Components Route Internationalization

Scope: applies to `packages/ui/src/components/blocks/**`.

These rules are ALWAYS ACTIVE for all status block presentation components and status utility modules rendering user-facing text or formatted dates.

### Rules

- **R-STAT-001** MUST: Status block presentation components MUST route all internationalization, localized strings, and text translations through @openstatus/ui/components/blocks/status-i18n.

### Verify

```bash
# Discover and execute repository linting and static analysis scripts to verify that status block components import localization resources exclusively through @openstatus/ui/components/blocks/status-i18n.
# Discover and run module dependency boundary validation checks to ensure no hardcoded string literals or unauthorized localization imports exist in status block components.
```

**Accept when:**
- All status block presentation components source localized strings exclusively from @openstatus/ui/components/blocks/status-i18n
- Static analysis and verification suites pass without detecting inline hardcoded text literals or unauthorized localization imports in status block components
- Dependency boundary verification confirms zero unapproved localization dependencies imported across block components

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>