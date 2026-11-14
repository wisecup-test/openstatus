---
version: 2
name: cross-cutting-components-routing-handlers-not-define-cafd
description: "Adoption of @openstatus/locales for Shared Localization and Internationalization Configuration: Components Routing Handlers Not Define Hoc"
metadata:
  actual:
    adr_id: cafd1745-b9ef-431b-8546-e3b2926031e0
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

# Adoption of @openstatus/locales for Shared Localization and Internationalization Configuration: Components Routing Handlers Not Define Hoc

Scope: applies to `apps/status-page/src/components/**`, `apps/status-page/src/content/markdown/**`.

These rules are ALWAYS ACTIVE for application features spanning route matching, user interface presentation, localized route matching, locale resolution, UI components, and context providers requiring localized date formatting or calendar displays.

### Rules

- **R-LOCALE-001** MUST_NOT: Components and routing handlers must not define ad-hoc locale lists, hardcoded language constants, or local date formatting locale mappings outside the @openstatus/locales package.

### Verify

```bash
# Discover and run the project static type checker across workspace packages to verify locale import compatibility.
# Discover and execute the project linter and dependency boundary checks to confirm all locale references route through the shared locale package.
# Discover and run the workspace test suite to validate locale resolution and date formatting behavior.
```

**Accept when:**
- The project static analysis and type checking suite passes with zero errors referencing @openstatus/locales imports.
- All route matching and date provider configurations resolve locale definitions from @openstatus/locales without duplicate locale definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>