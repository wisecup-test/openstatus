---
version: 2
name: cross-cutting-visual-icon-elements-receive-accessible-4698
description: "Adoption of @openstatus/icons for Application Iconography: Visual Icon Elements Receive Accessible Labels"
metadata:
  actual:
    adr_id: 46982d31-8055-4e15-8089-cc434a9620c5
    status: accepted
    candidate_group_id: 189c3723-9793-4c50-9b60-a1dd4ce669b2
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
      - src/components/**
      - src/data/**
---

# Adoption of @openstatus/icons for Application Iconography: Visual Icon Elements Receive Accessible Labels

Scope: applies to `apps/status-page/src/app/**`, `apps/status-page/src/components/**`, `apps/status-page/src/data/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OPN-001** SHOULD: Visual icon elements SHOULD receive accessible labels and appropriate sizing properties defined by design token conventions.
- **R-OPN-002** MANDATORY: Import icon components directly as named exports from @openstatus/icons to enable efficient tree-shaking during client bundle assembly.
- **R-OPN-003** MANDATORY: Verify that accessibility attributes such as aria-hidden or accessible title tags are applied based on whether the icon is decorative or interactive.

### Verify

```bash
# Discover and execute the workspace static analysis, linting, and type checking commands from the dependency manifest
# to verify icon imports originate from @openstatus/icons and conform to library definitions.
```

**Accept when:**
- All client components and client data modules requiring iconography resolve their icon symbols exclusively from @openstatus/icons.
- Static verification and dependency analysis execute without reporting missing icon bindings or undeclared icon packages.
- The resolved package version in the dependency resolution artifact matches the verified interface signatures used across consuming modules.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>