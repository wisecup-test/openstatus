---
version: 2
name: cross-cutting-status-block-components-not-embed-5b6a
description: "Adoption of @openstatus/ui/components/blocks/status-i18n for Status Block Localization: Status Block Components Not Embed Hardcoded"
metadata:
  actual:
    adr_id: 5b6a16eb-10ce-409d-8b62-81dd777d720a
    status: accepted
    candidate_group_id: b9b70b53-2d67-492e-a095-8040301ca6c7
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/blocks/**
---

# Adoption of @openstatus/ui/components/blocks/status-i18n for Status Block Localization: Status Block Components Not Embed Hardcoded

Scope: applies to `packages/ui/src/components/blocks/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-STATUS-001** MUST_NOT: Status block components MUST NOT embed hardcoded user-facing string literals directly within component templates or presentation markup.

### Verify

```bash
# Discover and execute repository linting and static analysis scripts to verify that status block components import localization resources exclusively through @openstatus/ui/components/blocks/status-i18n.
# Discover and run module dependency boundary validation checks to ensure no hardcoded string literals or unauthorized localization imports exist in status block components.
npm run lint:status-blocks || npx eslint status-blocks/
```

**Accept when:**
- All status block presentation components source localized strings exclusively from @openstatus/ui/components/blocks/status-i18n
- Static analysis and verification suites pass without detecting inline hardcoded text literals or unauthorized localization imports in status block components
- Dependency boundary verification confirms zero unapproved localization dependencies imported across block components

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration will fail on unauthorized localization imports or hardcoded user-facing strings.
</enforcement>