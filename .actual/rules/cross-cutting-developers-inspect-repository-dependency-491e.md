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

- **R-LOC-001** MUST: Developers MUST inspect the repository dependency resolution artifact to resolve exact locked versions and workspace module exports before consuming or modifying localization dependencies.
- **R-LOC-002** MUST: All status block presentation components MUST source localized strings exclusively from `@openstatus/ui/components/blocks/status-i18n`.
- **R-LOC-003** MUST: Maintain type definitions for translation keys to ensure compile-time verification across all referencing status blocks.
- **R-LOC-004** MUST: Coordinate all component string keys and formatting parameters with the interface exposed by the `status-i18n` module.

### Verify

```bash
# Discover and execute repository linting and static analysis scripts to verify status block components
if npm run --help | grep -q "lint"; then
  npm run lint
elif yarn --help > /dev/null 2>&1 && yarn run | grep -q "lint"; then
yarn lint
else
  npx eslint .
fi

# Run module dependency boundary validation checks to ensure no hardcoded string literals or unauthorized localization imports exist
if [ -f "package.json" ]; then
  grep -rn "@openstatus/ui/components/blocks/status-i18n" src/ || echo "Check import references"
fi
```

**Accept when:**
- All status block presentation components source localized strings exclusively from `@openstatus/ui/components/blocks/status-i18n`
- Static analysis and verification suites pass without detecting inline hardcoded text literals or unauthorized localization imports in status block components
- Dependency boundary verification confirms zero unapproved localization dependencies imported across block components

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration will fail on unauthorized localization imports or hardcoded user-facing strings.
</enforcement>