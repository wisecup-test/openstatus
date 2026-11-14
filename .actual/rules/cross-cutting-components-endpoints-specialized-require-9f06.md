---
version: 2
name: cross-cutting-components-endpoints-specialized-require-9f06
description: "Adoption of Internal utils Module for Shared Core Utilities: Components Endpoints Specialized Requirements Define Local"
metadata:
  actual:
    adr_id: 9f06d702-c08d-4eb2-9a07-cd2dea6198d3
    status: accepted
    candidate_group_id: 2606674d-2b12-4407-97f5-201fea1d93af
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/**
---

# Adoption of Internal utils Module for Shared Core Utilities: Components Endpoints Specialized Requirements Define Local

Scope: applies to `apps/web/src/app/api/**`.

These rules are ALWAYS ACTIVE for all API route handlers, server-rendered components, and visual generation subtrees requiring cross-cutting utility functions and shared presentation/layout components.

### Rules

- **R-UTIL-001** MAY: Components and endpoints with specialized requirements MAY define local helper functions provided those functions depend strictly on domain-specific structures not shared across boundaries.
- **R-UTIL-002** MANDATORY: Developers must ensure all exported routines in the internal utility module remain pure and free from side effects to maintain safe reusability across both client components and server route handlers.
- **R-UTIL-003** MANDATORY: Domain-specific utility logic must reside within corresponding domain or feature directories rather than accumulating in the shared core utility module.
- **R-UTIL-004** MANDATORY: Prior to writing code that uses a versioned library, developers must execute lock-version grounding (find dependency manifest, identify build tool, inspect repository lock/resolution artifact for exact version, look up official documentation, confirm API existence, and re-run per dependency at point of use).

### Verify

```bash
# Discover and run the project static analysis and linting scripts to verify compliance with internal module import conventions.
# Discover and execute the project unit test suite targeting the shared utility module to verify pure function contracts.
```

**Accept when:**
- All API routes and rendering components import shared utilities exclusively from the centralized internal utils module with zero linting violations.
- All test suites for the shared utility module pass completely without errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated CI pipeline linting/static analysis, peer code review, and architectural exception approval process for localized utilities.
</enforcement>