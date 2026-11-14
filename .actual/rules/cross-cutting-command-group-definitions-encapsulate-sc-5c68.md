---
version: 2
name: cross-cutting-command-group-definitions-encapsulate-sc-5c68
description: "Adoption of @openstatus/api Client Module for Command Menu Domain Operations: Command Group Definitions Encapsulate Scope Specific"
metadata:
  actual:
    adr_id: 5c689f76-4536-48da-a6a6-5d909599faeb
    status: accepted
    candidate_group_id: 1c3677cf-a2cc-42f7-ae4b-7c57addf2a75
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/command-menu/groups/**
---

# Adoption of @openstatus/api Client Module for Command Menu Domain Operations: Command Group Definitions Encapsulate Scope Specific

Scope: applies to `apps/dashboard/src/components/command-menu/groups/**`.

These rules are ALWAYS ACTIVE for dashboard command menu components, navigation group modules, and frontend navigation scopes requiring access to domain entity definitions and remote services.

### Rules

- **R-CMD-001** SHOULD: Command group definitions SHOULD encapsulate scope-specific constants and static navigation structures alongside domain models provided by @openstatus/api.
- **R-CMD-002** MANDATORY: Ensure command navigation scope modules import resource types directly from @openstatus/api and co-locate scope-specific static configurations.
- **R-CMD-003** MANDATORY: Implement in-memory lookup maps to resolve entity titles and navigation paths without triggering duplicate remote queries during command filtering.

### Verify

```bash
# Discover and run the project static analysis and linting scripts to verify module dependency boundaries
# Execute the repository type-checking script across dashboard workspace packages
# Run the project test suite to verify command group lookups and cache resolutions
```

**Accept when:**
- Static analysis validates that command menu modules consume @openstatus/api without unauthorized ad-hoc network clients.
- Type validation succeeds with zero type errors across all command menu navigation groups.
- Automated tests pass, verifying correct entity navigation and identifier lookups.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration builds fail upon detection of unauthorized direct network clients or unverified dependencies. Pull requests containing duplicated domain models or protocol divergence are blocked pending alignment with @openstatus/api.
</enforcement>