---
version: 2
name: cross-cutting-status-block-utility-functions-date-c363
description: "Adoption of @openstatus/ui/components/blocks/status-i18n for Status Block Localization: Status Block Utility Functions Date Formatting"
metadata:
  actual:
    adr_id: c36313d1-6b3d-4900-8057-a470c5ed2d25
    status: accepted
    candidate_group_id: b9b70b53-2d67-492e-a095-8040301ca6c7
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/blocks/**
---

# Adoption of @openstatus/ui/components/blocks/status-i18n for Status Block Localization: Status Block Utility Functions Date Formatting

Scope: applies to `packages/ui/src/components/blocks/**`.

These rules are ALWAYS ACTIVE for all status block utility functions, date formatting helpers, and status block UI components rendering user-facing text or formatted dates.

### Rules

- **R-STAT-001** SHOULD: Status block utility functions and date formatting helpers SHOULD coordinate with `@openstatus/ui/components/blocks/status-i18n` to ensure locale-aware presentation consistency.
- **R-STAT-002** MANDATORY: Status block UI components and status utility modules rendering user-facing text or formatted dates MUST source localized strings exclusively from `@openstatus/ui/components/blocks/status-i18n` and avoid hardcoded inline string literals or unauthorized localization imports.

### Verify

```bash
# Discover and execute repository linting and static analysis scripts to verify status block component imports and dependencies
npm run lint
# Run module dependency boundary validation checks to ensure no hardcoded string literals or unauthorized localization imports exist
npm run test:boundaries
```

**Accept when:**
- All status block presentation components source localized strings exclusively from `@openstatus/ui/components/blocks/status-i18n`
- Static analysis and verification suites pass without detecting inline hardcoded text literals or unauthorized localization imports in status block components
- Dependency boundary verification confirms zero unapproved localization dependencies imported across block components

<enforcement>
Claude Code MUST NOT skip or defer verification. All status block localization rules are strictly enforced via automated linting, module boundary checks, and pull request reviews.
</enforcement>