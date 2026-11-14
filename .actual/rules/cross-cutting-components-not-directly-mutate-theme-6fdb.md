---
version: 2
name: cross-cutting-components-not-directly-mutate-theme-6fdb
description: "Adoption of @openstatus/theme-store for Theme State Management: Components Not Directly Mutate Theme Storage"
metadata:
  actual:
    adr_id: 6fdbaae6-b684-42aa-b0c4-0125c079b8e2
    status: accepted
    candidate_group_id: 084c8d7c-08dd-49c7-b2dd-833dba6a38cf
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Adoption of @openstatus/theme-store for Theme State Management: Components Not Directly Mutate Theme Storage

Scope: applies to `apps/status-page/src/app/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TSM-001** SHOULD_NOT: Components SHOULD NOT directly mutate theme storage state outside established store actions and provider boundaries.
- **R-TSM-002** MANDATORY: Integrate ThemeProvider at the highest common layout level within route hierarchies to ensure uniform theme propagation.
- **R-TSM-003** MANDATORY: Expose theme values and updater actions exclusively through defined store interfaces to prevent hydration inconsistencies.

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