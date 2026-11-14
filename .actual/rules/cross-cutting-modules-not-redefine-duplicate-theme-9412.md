---
version: 2
name: cross-cutting-modules-not-redefine-duplicate-theme-9412
description: "Adoption of @openstatus/theme-store for Theme Schema Definition: Modules Not Redefine Duplicate Theme Variable"
metadata:
  actual:
    adr_id: 9412e717-1dc0-41f7-a10c-2f2a98ce8017
    status: accepted
    candidate_group_id: 953354fc-1228-4815-8dc1-ee03dbe3f2ef
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/pages/**
---

# Adoption of @openstatus/theme-store for Theme Schema Definition: Modules Not Redefine Duplicate Theme Variable

Scope: applies to `packages/db/src/schema/pages/**`.

These rules are ALWAYS ACTIVE for database persistence schemas, API validation layers, and internal packages that define entities configurable by light and dark mode styling variables.

### Rules

- **R-THEME-001** SHOULD_NOT: Modules MUST_NOT redefine or duplicate theme variable shape definitions outside of @openstatus/theme-store.

### Verify

```bash
# Discover and run the project static analysis and linting scripts to verify that theme references resolve to the shared module.
# Discover and execute test suites targeting database and validation schemas to confirm proper theme schema enforcement.
# Inspect the workspace package dependency graph to ensure no circular dependencies exist with the shared theme package.
```

**Accept when:**
- All schema and validation declarations requiring theme configuration import directly from @openstatus/theme-store.
- Repository static analysis and dependency graph audits confirm zero duplicate theme schema definitions across packages.
- Validation and schema tests verify that invalid theme variable structures are rejected at runtime.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated module boundary linting and dependency tree verification enforce these constraints during continuous integration.
</enforcement>