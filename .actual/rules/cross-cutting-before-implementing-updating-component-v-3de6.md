---
version: 2
name: cross-cutting-before-implementing-updating-component-v-3de6
description: "Adoption of class-variance-authority for Component Variant Styling: Before Implementing Updating Component Variant Definitions"
metadata:
  actual:
    adr_id: 3de698c3-d9a3-4eb2-8389-cad5ac3ad9de
    status: accepted
    candidate_group_id: a937ff75-96f0-4a05-a5bd-101569899b32
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/components/**
---

# Adoption of class-variance-authority for Component Variant Styling: Before Implementing Updating Component Variant Definitions

Scope: applies to `apps/status-page/src/components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CVA-001** MUST: Before implementing or updating component variant definitions, developers MUST inspect the repository lock artifact to determine the exact resolved version of class-variance-authority and verify API compatibility against that version.

### Verify

```bash
# 1. Discover the project manifest and lock file, check resolved class-variance-authority version
# 2. Run test suite (discover test runner from manifest)
npm test
# 3. Run static analysis / type check (discover linter/typechecker from manifest)
npm run lint
```

**Accept when:**
- All component test suites pass verifying correct styling application across all declared variant combinations.
- Static analysis and type checking complete with zero errors related to variant schema configurations.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>