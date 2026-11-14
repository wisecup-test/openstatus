---
version: 2
name: cross-cutting-command-menu-navigation-groups-scoping-540d
description: "Adoption of @openstatus/api Client Module for Command Menu Domain Operations: Command Menu Navigation Groups Scoping Modules"
metadata:
  actual:
    adr_id: 540d3bdb-6981-4aeb-b91f-401f728ee282
    status: accepted
    candidate_group_id: 1c3677cf-a2cc-42f7-ae4b-7c57addf2a75
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/command-menu/groups/**
---

# Adoption of @openstatus/api Client Module for Command Menu Domain Operations: Command Menu Navigation Groups Scoping Modules

Scope: applies to `apps/dashboard/src/components/command-menu/groups/**`.

These rules are ALWAYS ACTIVE for all dashboard command menu components, navigation group modules, and frontend navigation scopes requiring access to domain entity definitions and remote services.

### Rules

- **R-OPENSTATUS-001** MUST: Command menu navigation groups and scoping modules MUST consume domain entity contracts and service definitions through the @openstatus/api client module.

### Verify

```bash
# Discover and run project static analysis, linting, type-checking, and tests
# 1. Locate and execute workspace lint/static analysis scripts
# 2. Run repository type-checking script across dashboard workspace packages
# 3. Run project test suite to verify command menu group lookups and cache resolutions
```

**Accept when:**
- Static analysis validates that command menu modules consume @openstatus/api without unauthorized ad-hoc network clients.
- Type validation succeeds with zero type errors across all command menu navigation groups.
- Automated tests pass, verifying correct entity navigation and identifier lookups.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration builds fail upon detection of unauthorized direct network clients or unverified dependencies. Pull requests containing duplicated domain models or protocol divergence are blocked pending alignment with @openstatus/api.
</enforcement>