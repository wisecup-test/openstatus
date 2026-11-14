---
version: 2
name: cross-cutting-typescript-boundary-types-domain-contrac-b4b0
description: "Adoption of Zod for Runtime Schema Validation and Ingestion Boundary Modeling: Typescript Boundary Types Domain Contracts Derived"
metadata:
  actual:
    adr_id: b4b06d4b-b49c-4b35-9064-eff7215f8648
    status: accepted
    candidate_group_id: e31ac072-3db1-4961-a5ee-464d55ca9a14
    evidence:
      source: deep
  scope:
    root: packages/tinybird
    paths:
      - src/**
---

# Adoption of Zod for Runtime Schema Validation and Ingestion Boundary Modeling: Typescript Boundary Types Domain Contracts Derived

Scope: applies to `packages/tinybird/src/**`.

These rules are ALWAYS ACTIVE for all external data ingestion interfaces, telemetry schemas, audit log event contracts, and analytics client configuration parsing.

### Rules

- **R-BND-001** SHOULD: TypeScript boundary types and domain contracts SHOULD be derived directly from validation schemas through type inference rather than maintained as separate manual type declarations.

### Verify

```bash
# Discover and run the project repository test runner and static analysis type checking
# (e.g., npm test, npm run typecheck)
```

**Accept when:**
- All external ingestion payloads and client configuration objects pass validation against their respective schemas with zero unhandled schema parse exceptions.
- Audit log event schemas enforce discriminated string literal action types with matching structured metadata properties across test suites.
- Dynamic timestamp attributes evaluate at invocation time rather than schema module initialization time.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory for all changes touching boundary ingestion schemas and TypeScript types.
</enforcement>