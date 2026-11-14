---
version: 2
name: cross-cutting-applications-requiring-theme-state-synch-3794
description: "Adoption of @openstatus/theme-store for Theme State Management: Applications Requiring Theme State Synchronization Across"
metadata:
  actual:
    adr_id: 379460c6-47b9-4320-b130-13dbd0c44c1f
    status: accepted
    candidate_group_id: 084c8d7c-08dd-49c7-b2dd-833dba6a38cf
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Adoption of @openstatus/theme-store for Theme State Management: Applications Requiring Theme State Synchronization Across

Scope: applies to `apps/status-page/src/app/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-THEME-001** MUST: Applications requiring theme state synchronization across routing layouts MUST consume theme state through the @openstatus/theme-store module.

### Verify

```bash
# Discover repository verification scripts from the project manifest and execute the test suite
# Query the workspace dependency resolution artifact to verify that @openstatus/theme-store resolves correctly
# Discover and execute the project linting and module boundary checks across layout components
```

**Accept when:**
- The workspace dependency resolution artifact confirms that @openstatus/theme-store is resolved to a single consistent version across all consuming layouts.
- Layout components successfully coordinate ThemeProvider with state derived from @openstatus/theme-store without introducing uncoordinated local theme state.
- All automated project validation scripts complete without reporting module boundary or theme state synchronization errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>