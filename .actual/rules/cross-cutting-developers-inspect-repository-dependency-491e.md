---
version: 2
name: cross-cutting-developers-inspect-repository-dependency-491e
description: "Adoption of @openstatus/ui/components/blocks/status-i18n for Status Block Localization: Developers Inspect Repository Dependency Resolution Artifact"
metadata:
  actual:
    adr_id: 491efad7-bef4-469b-912d-334cda0366b3
    status: accepted
    candidate_group_id: b9b70b53-2d67-492e-a095-8040301ca6c7
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/blocks/**
---

# Adoption of @openstatus/ui/components/blocks/status-i18n for Status Block Localization: Developers Inspect Repository Dependency Resolution Artifact

Scope: applies to `packages/ui/src/components/blocks/**`.

These rules are ALWAYS ACTIVE for all status block UI components and status utility modules rendering user-facing text or formatted dates.

### Rules

- **R-STAT-001** MUST: Developers MUST inspect the repository dependency resolution artifact to resolve exact locked versions and workspace module exports before consuming or modifying localization dependencies.
- **R-STAT-002** MUST: Source all localized strings and timestamps exclusively through `@openstatus/ui/components/blocks/status-i18n`.

### Verify

```bash
# Discover and execute repository linting and static analysis scripts to verify status block component imports
# Discover and run module dependency boundary validation checks
```

**Accept when:**
- All status block presentation components source localized strings exclusively from @openstatus/ui/components/blocks/status-i18n
- Static analysis and verification suites pass without detecting inline hardcoded text literals or unauthorized localization imports in status block components
- Dependency boundary verification confirms zero unapproved localization dependencies imported across block components

<enforcement>
Claude Code MUST NOT skip or defer verification. All status block localization implementations must comply with centralized resolution and dependency checks.
</enforcement>