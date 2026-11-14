---
version: 2
name: cross-cutting-before-implementing-updating-schema-vali-dacf
description: "Zod Runtime Schema Validation for API Procedure Boundaries: Before Implementing Updating Schema Validations Consumer"
metadata:
  actual:
    adr_id: dacfd807-050e-4049-865b-977fe8ed1570
    status: accepted
    candidate_group_id: 0cbb6056-fd7d-4539-b46b-da94fc5fa10f
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/**
---

# Zod Runtime Schema Validation for API Procedure Boundaries: Before Implementing Updating Schema Validations Consumer

Scope: applies to `packages/api/src/**`.

These rules are ALWAYS ACTIVE for all public and protected procedure definitions accepting parameters from external callers, and the runtime parsing of dynamic configuration objects and deserialized entity attributes.

### Rules

- **R-ZOD-001** MUST: Before implementing or updating schema validations, inspect the repository lock artifact to determine the exact resolved version of Zod and verify that all utilized schema methods exist in that version's official API reference.

### Verify

```bash
# Discover and execute the project build and type verification script from the project manifest
# Example: npm run build / tsc --noEmit

# Discover and execute the repository automated test runner
# Example: npm test

# Discover and run the project static analysis linter
# Example: npm run lint
```

**Accept when:**
- All public and protected procedure definitions declare explicit input validation schemas prior to invoking service handlers.
- Type checking and automated test suites confirm schema compilation and successful rejection of malformed payloads.
- Boundary parser rejections return structured client errors without propagating unvalidated state to domain functions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis and type checking must be performed.
</enforcement>