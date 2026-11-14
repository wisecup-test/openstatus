---
version: 2
name: cross-cutting-navigation-components-not-define-duplica-3ad3
description: "Adoption of @openstatus/api Client Module for Command Menu Domain Operations: Navigation Components Not Define Duplicated Domain"
metadata:
  actual:
    adr_id: 3ad34693-b5e1-4bda-b651-c236ea945b20
    status: accepted
    candidate_group_id: 1c3677cf-a2cc-42f7-ae4b-7c57addf2a75
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/command-menu/groups/**
---

# Adoption of @openstatus/api Client Module for Command Menu Domain Operations: Navigation Components Not Define Duplicated Domain

Scope: applies to `apps/dashboard/src/components/command-menu/groups/**`.

These rules are ALWAYS ACTIVE for all dashboard command menu components, navigation group modules, and frontend navigation scopes requiring access to domain entity definitions and remote services.

### Rules

- **R-NAV-001** MUST_NOT: UI navigation components MUST NOT define duplicated domain entity schemas or independent network client instances for resources exposed by @openstatus/api.
- **R-NAV-002** MANDATORY: Consumers MUST derive tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-NAV-003** MANDATORY: Before writing code that uses a versioned library, execute the lock-version grounding sequence (find manifest, identify build tool, inspect lock/resolution artifact, look up official documentation for that exact version, confirm API existence, re-run per dependency at point of use).
- **R-NAV-004** MANDATORY: Ensure command navigation scope modules import resource types directly from @openstatus/api and co-locate scope-specific static configurations.
- **R-NAV-005** MANDATORY: Implement in-memory lookup maps to resolve entity titles and navigation paths without triggering duplicate remote queries during command filtering.

### Verify

```bash
# Discover and run project static analysis and linting scripts to verify module imports
# Execute repository type-checking script across dashboard workspace packages
# Run project test suite to verify command menu group lookups and cache resolutions
```

**Accept when:**
- Static analysis validates that command menu modules consume @openstatus/api without unauthorized ad-hoc network clients.
- Type validation succeeds with zero type errors across all command menu navigation groups.
- Automated tests pass, verifying correct entity navigation and identifier lookups.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration builds fail upon detection of unauthorized direct network clients or unverified dependencies. Pull requests containing duplicated domain models or protocol divergence are blocked pending alignment with @openstatus/api.
</enforcement>