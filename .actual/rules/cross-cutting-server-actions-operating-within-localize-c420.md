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

These rules are ALWAYS ACTIVE for server-rendered route segments, layouts, request configuration handlers, and server actions executing within localized route scopes.

### Rules

- **R-INTL-001** MUST: Server actions operating within localized route scopes MUST resolve locale-aware messages and routing redirects exclusively through next-intl/server.

### Verify

```bash
# Discover and run the project test runner script defined in the repository configuration
# Discover and execute the repository type-checking script
# Discover and run the repository linter script
```

**Accept when:**
- All localized route layout test fixtures and server action verification suites pass without error.
- Static analysis and type checking validate that all server-side localization helpers resolve strictly against next-intl/server.
- Continuous integration pipelines report clean verification runs with no conflicting internationalization dependencies detected.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests touching localized route trees or server actions must pass automated type checking, test suites, and code review verification.
</enforcement>