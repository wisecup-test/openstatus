---
version: 2
name: cross-cutting-developers-not-place-domain-specific-db6f
description: "Adoption of Internal utils Module for Shared Core Utilities: Developers Not Place Domain Specific Business"
metadata:
  actual:
    adr_id: db6fc614-1b04-4aa1-885b-086f0d11ddcd
    status: accepted
    candidate_group_id: 2606674d-2b12-4407-97f5-201fea1d93af
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/**
---

# Adoption of Internal utils Module for Shared Core Utilities: Developers Not Place Domain Specific Business

Scope: applies to `apps/web/src/app/api/**`.

These rules are ALWAYS ACTIVE for all API route handlers, server-rendered components, and visual generation subtrees requiring cross-cutting utility functions.

### Rules

- **R-UTIL-001** SHOULD_NOT: Developers SHOULD_NOT place domain-specific business rules, database queries, or external network client invocations within the core utils module.

### Verify

```bash
# Discover and run the project static analysis and linting scripts to verify compliance with internal module import conventions.
# Discover and execute the project unit test suite targeting the shared utility module to verify pure function contracts.
```

**Accept when:**
- All API routes and rendering components import shared utilities exclusively from the centralized internal utils module with zero linting violations.
- All test suites for the shared utility module pass completely without errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification of static analysis, linting, and unit test suites is mandatory.
</enforcement>