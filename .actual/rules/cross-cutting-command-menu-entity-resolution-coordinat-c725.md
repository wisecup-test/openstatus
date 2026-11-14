---
version: 2
name: cross-cutting-command-menu-entity-resolution-coordinat-c725
description: "Adoption of @openstatus/api Client Module for Command Menu Domain Operations: Command Menu Entity Resolution Coordinate Localized"
metadata:
  actual:
    adr_id: c725d060-c6de-4bd5-a68f-b9b86e334fad
    status: accepted
    candidate_group_id: 1c3677cf-a2cc-42f7-ae4b-7c57addf2a75
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/command-menu/groups/**
---

# Adoption of @openstatus/api Client Module for Command Menu Domain Operations: Command Menu Entity Resolution Coordinate Localized

Scope: applies to `apps/dashboard/src/components/command-menu/groups/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CMD-001** SHOULD: Command menu entity resolution SHOULD coordinate localized cache lookup structures with entity identifiers retrieved from @openstatus/api contracts.

### Verify

```bash
# Discover and run the project static analysis and linting scripts to verify that command menu scoping modules only import authorized domain client packages.
# Execute the repository type-checking script across dashboard workspace packages to confirm type compatibility with @openstatus/api exports.
# Run the project test suite to verify that command menu group lookups and cache resolutions operate correctly.
```

**Accept when:**
- Static analysis validates that command menu modules consume @openstatus/api without unauthorized ad-hoc network clients.
- Type validation succeeds with zero type errors across all command menu navigation groups.
- Automated tests pass, verifying correct entity navigation and identifier lookups.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>