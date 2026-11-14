---
version: 2
name: cross-cutting-shared-theme-state-access-outside-d1b2
description: "Adoption of @openstatus/theme-store for Theme State Management: Shared Theme State Access Outside Layout"
metadata:
  actual:
    adr_id: d1b2bf89-69a9-42a0-b026-8aed0ed1b010
    status: accepted
    candidate_group_id: 084c8d7c-08dd-49c7-b2dd-833dba6a38cf
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Adoption of @openstatus/theme-store for Theme State Management: Shared Theme State Access Outside Layout

Scope: applies to `apps/status-page/src/app/**`.

These rules are ALWAYS ACTIVE for top-level layout components, route group layouts requiring synchronized theme persistence, and any files or modules interacting with theme state outside layout boundaries.

### Rules

- **R-THEME-001** SHOULD: Shared theme state access outside layout boundaries SHOULD be restricted to selectors and interfaces exported directly by @openstatus/theme-store.

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
Claude Code MUST NOT skip or defer verification. All changes must pass workspace static analysis, boundary verification, and peer code review ensuring theme state is imported exclusively from @openstatus/theme-store.
</enforcement>