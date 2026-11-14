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

These rules are ALWAYS ACTIVE for all status block UI components, status utility modules rendering user-facing text or formatted dates, and related files matching the configured scope.

### Rules

- **R-LOC-001** SHOULD: Status block utility functions and date formatting helpers SHOULD coordinate with @openstatus/ui/components/blocks/status-i18n to ensure locale-aware presentation consistency.
- **R-LOC-002** MANDATORY: The consumer MUST derive tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-LOC-003** MANDATORY: Before writing code that uses a versioned library, execute in order: find dependency manifest, identify build tool, inspect lock or resolution artifact for exact resolved version, look up official documentation for that exact version, confirm every API/class/function exists in that version's documentation, and re-verify per dependency at point of use for version-sensitive behavior.

### Verify

```bash
# Discover and execute repository linting and static analysis scripts to verify status block localization imports
if npm --version >/dev/null 2>&1; then
  npm run lint --if-present
  npm run test --if-present
elif yarn --version >/dev/null 2>&1; then
  yarn lint
  yarn test
elif pnpm --version >/dev/null 2>&1; then
  pnpm lint
  pnpm test
else
  echo "No recognized package manager found for verification scripts."
  exit 1
fi
```

**Accept when:**
- All status block presentation components source localized strings exclusively from @openstatus/ui/components/blocks/status-i18n
- Static analysis and verification suites pass without detecting inline hardcoded text literals or unauthorized localization imports in status block components
- Dependency boundary verification confirms zero unapproved localization dependencies imported across block components

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory and enforced via automated CI pipelines, static analysis, and peer reviews.
</enforcement>