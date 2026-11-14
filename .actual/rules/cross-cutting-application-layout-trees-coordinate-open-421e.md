---
version: 2
name: cross-cutting-application-layout-trees-coordinate-open-421e
description: "Adoption of @openstatus/theme-store for Theme State Management: Application Layout Trees Coordinate Openstatus Theme"
metadata:
  actual:
    adr_id: 421e8a3b-ae7f-4642-bc5a-c30a3bc110fa
    status: accepted
    candidate_group_id: 084c8d7c-08dd-49c7-b2dd-833dba6a38cf
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Adoption of @openstatus/theme-store for Theme State Management: Application Layout Trees Coordinate Openstatus Theme

Scope: applies to `apps/status-page/src/app/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-THEME-001** MUST: Application layout trees MUST coordinate @openstatus/theme-store with ThemeProvider at the top-level boundary rather than instantiating ad-hoc theme storage within child route components.

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