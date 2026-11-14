---
version: 2
name: cross-cutting-components-requiring-date-formatting-cal-80d0
description: "Adoption of @openstatus/locales for Shared Localization and Internationalization Configuration: Components Requiring Date Formatting Calendar Rendering"
metadata:
  actual:
    adr_id: 80d04066-1e1d-4eeb-ab78-e8f08772ba3b
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

# Adoption of @openstatus/locales for Shared Localization and Internationalization Configuration: Components Requiring Date Formatting Calendar Rendering

Scope: applies to `apps/status-page/src/components/**`, `apps/status-page/src/content/markdown/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-LOC-001** SHOULD: UI components requiring date formatting or calendar rendering should consume the localized date context configured via @openstatus/locales rather than importing external locale bundles independently.

### Verify

```bash
# Discover and run the project static type checker across workspace packages
# Discover and execute the project linter and dependency boundary checks
# Discover and run the workspace test suite to validate locale resolution
```

**Accept when:**
- The project static analysis and type checking suite passes with zero errors referencing @openstatus/locales imports.
- All route matching and date provider configurations resolve locale definitions from @openstatus/locales without duplicate locale definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration builds and automated static analysis enforce these rules.
</enforcement>