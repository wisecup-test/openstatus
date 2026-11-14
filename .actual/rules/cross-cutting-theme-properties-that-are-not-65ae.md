---
version: 2
name: cross-cutting-theme-properties-that-are-not-65ae
description: "Adoption of @openstatus/theme-store for Theme Schema Definition: Theme Properties That Are Not Mandatory"
metadata:
  actual:
    adr_id: 65ae3c61-565a-4baf-a274-fdf4d1658742
    status: accepted
    candidate_group_id: 953354fc-1228-4815-8dc1-ee03dbe3f2ef
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/pages/**
---

# Adoption of @openstatus/theme-store for Theme Schema Definition: Theme Properties That Are Not Mandatory

Scope: applies to `packages/db/src/schema/pages/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-THEME-001** SHOULD: Theme properties that are not mandatory for an entity configuration SHOULD be marked as optional while preserving the inner schema contract of the shared theme variable schema.
- **R-THEME-002** MANDATORY: Import theme variable schemas from @openstatus/theme-store when constructing composite entity schemas requiring visual styling configuration.
- **R-THEME-003** MANDATORY: Ensure optional light and dark mode configuration properties reuse the shared theme variable schema without redefining nested property constraints.

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
Claude Code MUST NOT skip or defer verification. Automated module boundary linting, dependency tree verification, and code review inspection are mandatory.
</enforcement>