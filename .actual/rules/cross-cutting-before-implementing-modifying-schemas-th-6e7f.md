---
version: 2
name: cross-cutting-before-implementing-modifying-schemas-th-6e7f
description: "Adoption of Zod for Runtime Schema Validation and Ingestion Boundary Modeling: Before Implementing Modifying Schemas That Use"
metadata:
  actual:
    adr_id: 6e7f937d-0010-46b3-bfca-00127bfd80d8
    status: accepted
    candidate_group_id: e31ac072-3db1-4961-a5ee-464d55ca9a14
    evidence:
      source: deep
  scope:
    root: packages/tinybird
    paths:
      - src/**
---

# Adoption of Zod for Runtime Schema Validation and Ingestion Boundary Modeling: Before Implementing Modifying Schemas That Use

Scope: applies to `packages/tinybird/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST: Before implementing or modifying schemas that use the validation library, developers MUST inspect the repository dependency lock artifact to determine the exact resolved library version and verify compatibility with its published reference documentation.

### Verify

```bash
# Discover the project repository test runner and execute automated test suites governing boundary schema validation and parsing contracts.
# Discover the project repository static analysis verification tools and execute type checking.
```

**Accept when:**
- All external ingestion payloads and client configuration objects pass validation against their respective Zod schemas with zero unhandled schema parse exceptions.
- Audit log event schemas enforce discriminated string literal action types with matching structured metadata properties across test suites.
- Dynamic timestamp attributes evaluate at invocation time rather than schema module initialization time.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated test suites and static analysis tools must validate payload acceptance, schema contracts, and type inference.
</enforcement>