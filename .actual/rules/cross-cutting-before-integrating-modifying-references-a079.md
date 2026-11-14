---
version: 2
name: cross-cutting-before-integrating-modifying-references-a079
description: "Adoption of @openstatus/icons for Application Iconography: Before Integrating Modifying References Openstatus Icons"
metadata:
  actual:
    adr_id: a079a249-6cdb-406b-8086-0b53b970e50d
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

# Adoption of @openstatus/icons for Application Iconography: Before Integrating Modifying References Openstatus Icons

Scope: applies to `apps/status-page/src/app/**`, `apps/status-page/src/components/**`, `apps/status-page/src/data/**`.

These rules are ALWAYS ACTIVE for all client-side user interface components rendering visual symbols, status badges, action controls, and client-side data definitions and descriptors that assign icon symbols to status categories or domain entities.

### Rules

- **R-OPN-001** MUST: Before integrating or modifying references to @openstatus/icons, developers MUST discover the workspace dependency manifest and resolution artifact to resolve the exact locked version.
- **R-OPN-002** MUST: Execute dependency resolution grounding steps in order (find dependency manifest, identify build tool, inspect repository lock/resolution artifact, look up official documentation for that exact version, confirm every API/class/function exists in that version before using, and re-run for version-sensitive behavior at point of use).
- **R-OPN-003** MUST: Import icon components directly as named exports from @openstatus/icons to enable efficient tree-shaking during client bundle assembly.
- **R-OPN-004** MUST: Verify that accessibility attributes such as aria-hidden or accessible title tags are applied based on whether the icon is decorative or interactive.

### Verify

```bash
# Discover the workspace static analysis and linting command from the dependency manifest and execute it to verify all icon imports originate from @openstatus/icons.
# Discover and run the project type checking script to validate that all icon component properties and imported symbols conform to the library definitions.
# Execute the workspace dependency audit script discovered from the root configuration to confirm no unauthorized external icon packages are declared in dependency manifests.
```

**Accept when:**
- All client components and client data modules requiring iconography resolve their icon symbols exclusively from @openstatus/icons.
- Static verification and dependency analysis execute without reporting missing icon bindings or undeclared icon packages.
- The resolved package version in the dependency resolution artifact matches the verified interface signatures used across consuming modules.

<enforcement>
Claude Code MUST NOT skip or defer verification. All client components requiring iconography must exclusively resolve symbols from @openstatus/icons with verified locked versions.
</enforcement>