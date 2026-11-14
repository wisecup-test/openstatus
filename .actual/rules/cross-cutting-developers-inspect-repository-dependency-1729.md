---
version: 2
name: cross-cutting-developers-inspect-repository-dependency-1729
description: "Adoption of @openstatus/api Client Module for Command Menu Domain Operations: Developers Inspect Repository Dependency Manifest Resolution"
metadata:
  actual:
    adr_id: 172942fe-2cb3-46f7-b772-2961fc89e6b0
    status: accepted
    candidate_group_id: 1c3677cf-a2cc-42f7-ae4b-7c57addf2a75
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/command-menu/groups/**
---

# Adoption of @openstatus/api Client Module for Command Menu Domain Operations: Developers Inspect Repository Dependency Manifest Resolution

Scope: applies to `apps/dashboard/src/components/command-menu/groups/**`.

These rules are ALWAYS ACTIVE for all dashboard command menu components, navigation group modules, and frontend navigation scopes requiring access to domain entity definitions and remote services.

### Rules

- **R-API-001** MUST: Developers MUST inspect the repository dependency manifest and resolution lock artifact to verify the exact resolved version of @openstatus/api before introducing new integrations.
- **R-API-002** MUST: Ensure command navigation scope modules import resource types directly from @openstatus/api and co-locate scope-specific static configurations.
- **R-API-003** MUST: Implement in-memory lookup maps to resolve entity titles and navigation paths without triggering duplicate remote queries during command filtering.

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
Claude Code MUST NOT skip or defer verification. Continuous integration builds and pull requests enforce module dependency boundaries, type safety, and contract adherence with @openstatus/api.
</enforcement>