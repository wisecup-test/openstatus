---
version: 2
name: cross-cutting-developers-import-shared-cross-cutting-130b
description: "Adoption of Internal utils Module for Shared Core Utilities: Developers Import Shared Cross Cutting Utility"
metadata:
  actual:
    adr_id: 130b576f-26cf-499f-8480-f44db2e2431a
    status: accepted
    candidate_group_id: 2606674d-2b12-4407-97f5-201fea1d93af
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/**
---

# Adoption of Internal utils Module for Shared Core Utilities: Developers Import Shared Cross Cutting Utility

Scope: applies to `apps/web/src/app/api/**`.

These rules are ALWAYS ACTIVE for all API route handlers, server-rendered components, and visual generation subtrees requiring cross-cutting utility functions.

### Rules

- **R-UTIL-001** MUST: Developers MUST import shared cross-cutting utility functions from the designated internal utils module rather than authoring duplicate helper routines within local component or route directories.

### Verify

```bash
# Discover and run project static analysis and linting scripts
# Discover and execute project unit test suite targeting the shared utility module
```

**Accept when:**
- All API routes and rendering components import shared utilities exclusively from the centralized internal utils module with zero linting violations.
- All test suites for the shared utility module pass completely without errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated continuous integration and peer code review.
</enforcement>