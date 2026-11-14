---
version: 2
name: cross-cutting-application-source-supported-locale-defi-bb7a
description: "Adoption of @openstatus/locales for Shared Localization and Internationalization Configuration: Application Source Supported Locale Definitions Route"
metadata:
  actual:
    adr_id: bb7a2d57-b639-4c37-96f7-c584a4aaee8c
    status: accepted
    candidate_group_id: afff3de8-6b16-4c07-818e-ae7a6482e6ed
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/components/**
      - src/content/markdown/**
---

# Adoption of @openstatus/locales for Shared Localization and Internationalization Configuration: Application Source Supported Locale Definitions Route

Scope: applies to `apps/status-page/src/components/**`, `apps/status-page/src/content/markdown/**`.

These rules are ALWAYS ACTIVE for all application routing logic, UI components, and context providers requiring localized string resolution, route matching, or date formatting.

### Rules

- **R-LOC-001** MUST: The application must source all supported locale definitions, route localization matching rules, and locale-specific provider configurations exclusively from @openstatus/locales.

### Verify

```bash
# Discover and run the project static type checker across workspace packages to verify locale import compatibility
# Discover and execute the project linter and dependency boundary checks to confirm all locale references route through the shared locale package
# Discover and run the workspace test suite to validate locale resolution and date formatting behavior
```

**Accept when:**
- The project static analysis and type checking suite passes with zero errors referencing @openstatus/locales imports.
- All route matching and date provider configurations resolve locale definitions from @openstatus/locales without duplicate locale definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated pull request dependency analysis, workspace linter boundaries, and continuous integration static type checking enforce these constraints.
</enforcement>