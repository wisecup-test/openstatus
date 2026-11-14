---
version: 2
name: cross-cutting-when-internal-utility-modules-interface-2f4a
description: "Adoption of Internal utils Module for Shared Core Utilities: When Internal Utility Modules Interface Versioned"
metadata:
  actual:
    adr_id: 2f4a9670-8e49-4622-b463-c16cb7108e43
    status: accepted
    candidate_group_id: 2606674d-2b12-4407-97f5-201fea1d93af
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/**
---

# Adoption of Internal utils Module for Shared Core Utilities: When Internal Utility Modules Interface Versioned

Scope: applies to `apps/web/src/app/api/**`.

These rules are ALWAYS ACTIVE for all API route handlers, server-rendered components, and visual generation subtrees requiring cross-cutting utility functions.

### Rules

- **R-UTIL-001** MUST: When internal utility modules interface with versioned third-party libraries, consumers MUST inspect the repository lock artifact to determine the exact resolved dependency version prior to implementation.

### Verify

```bash
# Discover and run the project static analysis and linting scripts
# Discover and execute the project unit test suite targeting the shared utility module
```

**Accept when:**
- All API routes and rendering components import shared utilities exclusively from the centralized internal utils module with zero linting violations.
- All test suites for the shared utility module pass completely without errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated continuous integration and peer code review.
</enforcement>