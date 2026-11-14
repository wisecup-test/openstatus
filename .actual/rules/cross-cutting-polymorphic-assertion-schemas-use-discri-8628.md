---
version: 2
name: cross-cutting-polymorphic-assertion-schemas-use-discri-8628
description: "Adopt Zod for Runtime Assertion Schema Validation and Parsing: Polymorphic Assertion Schemas Use Discriminated Literal"
metadata:
  actual:
    adr_id: 8628ee39-b964-453d-97a0-ad341182e2b1
    status: accepted
    candidate_group_id: 2433e36a-68e4-4bbc-93f7-a891b38dda85
    evidence:
      source: deep
  scope:
    root: packages/assertions
    paths:
      - src/**
---

# Adopt Zod for Runtime Assertion Schema Validation and Parsing: Polymorphic Assertion Schemas Use Discriminated Literal

Scope: applies to `packages/assertions/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST: Polymorphic assertion schemas MUST use discriminated literal types via z.literal on the type discriminator property.

### Verify

```bash
# Discover the test runner from the project manifest and execute the test suite covering assertion schema validation and serialization.
# Discover the static analysis tool from the project configuration and verify type compatibility between inferred schema types and assertion handlers.
```

**Accept when:**
- All assertion payload parsing tests pass without schema validation regressions.
- Static type checking completes with zero type errors across assertion schema definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>