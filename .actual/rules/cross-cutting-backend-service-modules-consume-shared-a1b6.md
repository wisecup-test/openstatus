---
version: 2
name: cross-cutting-backend-service-modules-consume-shared-a1b6
description: "Adoption of @openstatus/utils Core Utility Library: Backend Service Modules Consume Shared Cross"
metadata:
  actual:
    adr_id: a1b6a3e6-41e9-4f2c-9262-585d435ccd20
    status: accepted
    candidate_group_id: 24f82739-f335-4572-9c13-bfa2a59d6c93
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/monitor/**
      - src/page-subscriber/**
---

# Adoption of @openstatus/utils Core Utility Library: Backend Service Modules Consume Shared Cross

Scope: applies to `packages/services/src/monitor/**`, `packages/services/src/page-subscriber/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-UTIL-001** MUST: Backend service modules MUST consume shared cross-cutting helper functions and generic primitives through the `@openstatus/utils` internal module rather than reimplementing ad-hoc utility routines locally.

### Verify

```bash
# Discover and execute the repository static analysis, linting scripts, and test suites
npm run lint || pnpm lint || yarn lint
npm test || pnpm test || yarn test
```

**Accept when:**
- All service handlers importing shared utilities successfully resolve dependencies against the internal utility module without circular reference errors.
- Static analysis and repository validation test suites pass cleanly with no redundant local utility implementations.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests introducing duplicated general-purpose utilities or bypassing the shared utility module are blocked until refactored.
</enforcement>