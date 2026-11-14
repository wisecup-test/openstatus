---
version: 2
name: cross-cutting-any-extension-supported-application-lang-1f44
description: "Adoption of @openstatus/locales for Shared Localization and Internationalization Configuration: Any Extension Supported Application Languages Route"
metadata:
  actual:
    adr_id: 1f4406de-6729-4c4f-af8b-9060d67221f5
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

# Adoption of @openstatus/locales for Shared Localization and Internationalization Configuration: Any Extension Supported Application Languages Route

Scope: applies to `apps/status-page/src/components/**`, `apps/status-page/src/content/markdown/**`.

These rules are ALWAYS ACTIVE for all application routing logic, route matching, locale resolution, UI components, and context providers requiring localized date formatting or calendar displays.

### Rules

- **R-LOCALE-001** MUST: Introduce any extension to supported application languages or route localization parameters within @openstatus/locales to maintain cross-package consistency.

### Verify

```bash
# Discover and run the project static type checker across workspace packages
# Discover and execute the project linter and dependency boundary checks
# Discover and run the workspace test suite
```

**Accept when:**
- The project static analysis and type checking suite passes with zero errors referencing @openstatus/locales imports.
- All route matching and date provider configurations resolve locale definitions from @openstatus/locales without duplicate locale definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification. All locale definitions and internationalization configurations must be verified via automated static analysis, type checking, and linter boundary checks.
</enforcement>