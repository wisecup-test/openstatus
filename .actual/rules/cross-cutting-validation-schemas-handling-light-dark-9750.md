---
version: 2
name: cross-cutting-validation-schemas-handling-light-dark-9750
description: "Adoption of @openstatus/theme-store for Theme Schema Definition: Validation Schemas Handling Light Dark Mode"
metadata:
  actual:
    adr_id: 975091df-45e4-4220-8c81-7aac03420dbb
    status: accepted
    candidate_group_id: 953354fc-1228-4815-8dc1-ee03dbe3f2ef
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/pages/**
---

# Adoption of @openstatus/theme-store for Theme Schema Definition: Validation Schemas Handling Light Dark Mode

Scope: applies to `packages/db/src/schema/pages/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-THEME-001** MUST: Validation schemas handling light and dark mode theme variables MUST compose themeVarsSchema from the shared module to ensure structural consistency.

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
Claude Code MUST NOT skip or defer verification.
</enforcement>