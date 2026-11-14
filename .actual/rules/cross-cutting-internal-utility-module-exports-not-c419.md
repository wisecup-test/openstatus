---
version: 2
name: cross-cutting-internal-utility-module-exports-not-c419
description: "Adoption of Internal utils Module for Shared Core Utilities: Internal Utility Module Exports Not Introduce"
metadata:
  actual:
    adr_id: c419fab2-b020-429f-b251-80b8205c6f4d
    status: accepted
    candidate_group_id: 2606674d-2b12-4407-97f5-201fea1d93af
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/**
---

# Adoption of Internal utils Module for Shared Core Utilities: Internal Utility Module Exports Not Introduce

Scope: applies to `apps/web/src/app/api/**`.

These rules are ALWAYS ACTIVE for all API route handlers, server-rendered components, and visual generation subtrees requiring cross-cutting utility functions.

### Rules

- **R-UTIL-001** MUST_NOT: Internal utility module exports MUST_NOT introduce side effects, maintain mutable module-level state, or perform global runtime modifications.

### Verify

```bash
# Discover and run project static analysis and linting scripts
npm run lint
# Discover and execute project unit test suite targeting the shared utility module
npm test
```

**Accept when:**
- All API routes and rendering components import shared utilities exclusively from the centralized internal utils module with zero linting violations.
- All test suites for the shared utility module pass completely without errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. All changes modifying or adding exports to the shared utility module must be verified by automated linting and unit tests, and are subject to mandatory peer code review.
</enforcement>