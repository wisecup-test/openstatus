---
version: 2
name: cross-cutting-application-components-not-introduce-ind-63ea
description: "Adoption of @openstatus/theme-store for Theme State Management: Application Components Not Introduce Independent Local"
metadata:
  actual:
    adr_id: 63ea4982-137e-45d2-965f-232312c9c644
    status: accepted
    candidate_group_id: 084c8d7c-08dd-49c7-b2dd-833dba6a38cf
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Adoption of @openstatus/theme-store for Theme State Management: Application Components Not Introduce Independent Local

Scope: applies to `apps/status-page/src/app/**`.

These rules are ALWAYS ACTIVE for top-level layout components, route group layouts requiring synchronized theme persistence, and any related application presentation files within the configured scope.

### Rules

- **R-THEME-001** MUST_NOT: Application components MUST NOT introduce independent, local theme state stores that duplicate or circumvent @openstatus/theme-store.
- **R-THEME-002** MANDATORY: Integrate ThemeProvider at the highest common layout level within route hierarchies to ensure uniform theme propagation.
- **R-THEME-003** MANDATORY: Expose theme values and updater actions exclusively through defined store interfaces to prevent hydration inconsistencies.

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
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks, peer code reviews, and static analysis will block pull requests introducing divergent theme state storage or bypassing @openstatus/theme-store.
</enforcement>