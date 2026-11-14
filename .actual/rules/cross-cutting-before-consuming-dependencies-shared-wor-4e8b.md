---
version: 2
name: cross-cutting-before-consuming-dependencies-shared-wor-4e8b
description: "Adoption of @openstatus/db/src/schema in Notification Messaging Utilities: Before Consuming Dependencies Shared Workspace Modules"
metadata:
  actual:
    adr_id: 4e8bab35-eb36-428d-a2da-16fb0fa63798
    status: accepted
    candidate_group_id: dd0719a3-2e5f-40d7-89de-6853e51b15fd
    evidence:
      source: deep
  scope:
    root: packages/notifications/base
    paths:
      - src/utils/**
---

# Adoption of @openstatus/db/src/schema in Notification Messaging Utilities: Before Consuming Dependencies Shared Workspace Modules

Scope: applies to `packages/notifications/base/src/utils/**`.

These rules are ALWAYS ACTIVE for notification formatting utilities requiring database entity structure representations and workspace modules constructing message templates from shared schema models.

### Rules

- **R-DEP-001** MUST: Before consuming dependencies from shared workspace modules, the consumer MUST inspect the workspace dependency manifest and resolution artifacts to resolve the exact dependency constraints.
- **R-DEP-002** MUST: Derive all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-DEP-003** MUST: Execute lock-version grounding in order (find dependency manifest, identify build tool, inspect lock/resolution artifact, look up official documentation for that exact version, confirm every API/class/function exists, re-run per dependency at point of use) before writing code that uses a versioned library.
- **R-DEP-004** MUST: Ensure shared schema packages are declared as workspace dependencies rather than external registry dependencies.
- **R-DEP-005** MUST: Maintain schema consumption at the interface level to prevent notification templates from binding to transient database column conventions.

### Verify

```bash
# Discover and execute the workspace type-checker to verify cross-module schema import compatibility
# Discover and run the repository dependency boundary analyzer to confirm valid workspace package references
```

**Accept when:**
- Type validation succeeds with all imported schema structures resolving against shared definitions without compilation errors
- Workspace boundary checks confirm that module imports conform to package dependency rules

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated workspace build and static analysis checks, as well as peer architectural review.
</enforcement>