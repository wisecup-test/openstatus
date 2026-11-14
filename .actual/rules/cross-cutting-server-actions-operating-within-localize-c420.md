---
version: 2
name: cross-cutting-server-actions-operating-within-localize-c420
description: "next-intl/server Adoption for Server-Side Internationalization: Server Actions Operating Within Localized Route"
metadata:
  actual:
    adr_id: c420b2a2-0a68-40d0-a44a-9f82e17793ff
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

# next-intl/server Adoption for Server-Side Internationalization: Server Actions Operating Within Localized Route

Scope: applies to `apps/status-page/src/app/**`, `apps/status-page/src/i18n/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NSIS-001** MUST: Server actions operating within localized route scopes MUST resolve locale-aware messages and routing redirects exclusively through next-intl/server.

### Verify

```bash
# Discover and run the project test runner script defined in the repository configuration to execute all test suites covering localized layouts and server actions.
# Discover and execute the repository type-checking script to confirm full type compliance for server-side internationalization imports and request handlers.
# Discover and run the repository linter script to ensure no unauthorized internationalization imports exist across server modules.
```

**Accept when:**
- All localized route layout test fixtures and server action verification suites pass without error.
- Static analysis and type checking validate that all server-side localization helpers resolve strictly against next-intl/server.
- Continuous integration pipelines report clean verification runs with no conflicting internationalization dependencies detected.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>