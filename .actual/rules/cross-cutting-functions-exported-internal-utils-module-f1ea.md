---
version: 2
name: cross-cutting-functions-exported-internal-utils-module-f1ea
description: "Adoption of Internal utils Module for Shared Core Utilities: Functions Exported Internal Utils Module Pure"
metadata:
  actual:
    adr_id: f1eafc3f-2414-49a4-a822-7e329c993c10
    status: accepted
    candidate_group_id: 2606674d-2b12-4407-97f5-201fea1d93af
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/**
---

# Adoption of Internal utils Module for Shared Core Utilities: Functions Exported Internal Utils Module Pure

Scope: applies to `apps/web/src/app/api/**`.

These rules are ALWAYS ACTIVE for all API route handlers, server-rendered components, and visual generation subtrees requiring cross-cutting utility functions.

### Rules

- **R-UTIL-001** SHOULD: Functions exported by the internal utils module SHOULD be pure, deterministic functions that take explicit inputs and produce predictable outputs.
- **R-UTIL-002** MANDATORY: Developers must ensure all exported routines remain pure and free from side effects to maintain safe reusability across both client components and server route handlers.
- **R-UTIL-003** MANDATORY: Domain-specific utility logic must reside within corresponding domain or feature directories rather than accumulating in the shared core utility module.

### Verify

```bash
# Discover and run the project static analysis and linting scripts to verify compliance with internal module import conventions.
# Discover and execute the project unit test suite targeting the shared utility module to verify pure function contracts.
```

**Accept when:**
- All API routes and rendering components import shared utilities exclusively from the centralized internal utils module with zero linting violations.
- All test suites for the shared utility module pass completely without errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>