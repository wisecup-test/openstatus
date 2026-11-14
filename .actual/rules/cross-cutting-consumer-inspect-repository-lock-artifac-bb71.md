---
version: 2
name: cross-cutting-consumer-inspect-repository-lock-artifac-bb71
description: "next-intl/server Adoption for Server-Side Internationalization: Consumer Inspect Repository Lock Artifact Verify"
metadata:
  actual:
    adr_id: bb71642a-9607-40ec-99b2-f144fb3b605f
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

# next-intl/server Adoption for Server-Side Internationalization: Consumer Inspect Repository Lock Artifact Verify

Scope: applies to `apps/status-page/src/app/**`, `apps/status-page/src/i18n/**`.

These rules are ALWAYS ACTIVE for server-rendered route segments, layouts requiring localized content presentation, request configuration handlers, and server actions executing within localized route scopes.

### Rules

- **R-NSIS-001** MUST: The consumer MUST inspect the repository lock artifact and verify the exact resolved version of next-intl before implementing or modifying server-side localization code.

### Verify

```bash
# Discover and execute the project test runner script defined in the repository configuration
# Discover and execute the repository type-checking script
# Discover and execute the repository linter script
```

**Accept when:**
- All localized route layout test fixtures and server action verification suites pass without error.
- Static analysis and type checking validate that all server-side localization helpers resolve strictly against next-intl/server.
- Continuous integration pipelines report clean verification runs with no conflicting internationalization dependencies detected.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration pipelines and architecture/code reviews verify compliance; unapproved localization libraries or bypassed server request configurations fail verification and are blocked from merging.
</enforcement>