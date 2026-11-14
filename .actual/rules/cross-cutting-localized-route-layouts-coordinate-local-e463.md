---
version: 2
name: cross-cutting-localized-route-layouts-coordinate-local-e463
description: "next-intl/server Adoption for Server-Side Internationalization: Localized Route Layouts Coordinate Localization State"
metadata:
  actual:
    adr_id: e463f4e6-2a77-4024-ba89-94062e2b57d0
    status: accepted
    candidate_group_id: c77b2748-b572-41ef-9ec0-64178f3643db
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
      - src/i18n/**
---

# next-intl/server Adoption for Server-Side Internationalization: Localized Route Layouts Coordinate Localization State

Scope: applies to `apps/status-page/src/app/**`, `apps/status-page/src/i18n/**`.

These rules are ALWAYS ACTIVE for server-rendered route segments, layouts, request configuration handlers, and server actions requiring localized content presentation.

### Rules

- **R-INTL-001** MUST: Localized route layouts MUST coordinate localization state using server-side providers and request configuration established through next-intl/server.

### Verify

```bash
# Discover and run the project test runner script defined in the repository configuration
npm test
# Discover and execute the repository type-checking script
npm run typecheck
# Discover and run the repository linter script
npm run lint
```

**Accept when:**
- All localized route layout test fixtures and server action verification suites pass without error.
- Static analysis and type checking validate that all server-side localization helpers resolve strictly against next-intl/server.
- Continuous integration pipelines report clean verification runs with no conflicting internationalization dependencies detected.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration pipelines enforce type checking and localized test suites, and pull requests failing verification are blocked from merging.
</enforcement>