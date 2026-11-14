---
version: 2
name: cross-cutting-internationalization-request-configurati-1194
description: "next-intl/server Adoption for Server-Side Internationalization: Internationalization Request Configuration Isolate Locale Routing"
metadata:
  actual:
    adr_id: 119443e1-a378-4e57-98e9-fc52906dc828
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

# next-intl/server Adoption for Server-Side Internationalization: Internationalization Request Configuration Isolate Locale Routing

Scope: applies to `apps/status-page/src/app/**`, `apps/status-page/src/i18n/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-I18N-001** SHOULD: Internationalization request configuration SHOULD isolate locale routing definitions from message loading logic to preserve clean request-time execution boundaries.
- **R-I18N-002** MANDATORY: The consumer MUST discover tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-I18N-003** MANDATORY: Before writing code that uses a versioned library, execute lock-version grounding in order: find manifest, identify build tool, inspect lock artifact, look up official documentation for the exact version, and confirm every API exists in that exact version.
- **R-I18N-004** MANDATORY: Centralize internationalization request handling by defining a dedicated request configuration that derives locale parameters and loads corresponding translation messages before layout composition.
- **R-I18N-005** MANDATORY: Ensure server action routines obtain localized error formatting and redirection primitives from server-side internationalization exports rather than client-oriented hooks.

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
Claude Code MUST NOT skip or defer verification. Verified by automated CI pipelines, architecture reviews, and code review verification for pull requests touching localized route trees or server actions.
</enforcement>