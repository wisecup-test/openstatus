---
version: 2
name: cross-cutting-provider-integration-schemas-explicitly-841b
description: "Zod Schema Validation for External Provider Payloads: Provider Integration Schemas Explicitly Model Expected"
metadata:
  actual:
    adr_id: 841bbde9-5e8c-435c-9454-acaebf7f3608
    status: accepted
    candidate_group_id: 47a971ad-1e20-4235-b2c9-cbdf09abe622
    evidence:
      source: deep
  scope:
    root: packages/importers
    paths:
      - src/providers/**
---

# Zod Schema Validation for External Provider Payloads: Provider Integration Schemas Explicitly Model Expected

Scope: applies to `packages/importers/src/providers/**`.

These rules are ALWAYS ACTIVE for external provider integration clients making network requests to third-party endpoints and API response payload parsing and contract definition modules at the ingestion boundary.

### Rules

- **R-ZOD-001** MUST: Provider integration schemas MUST explicitly model expected properties, nullable fields, and enum values using Zod primitives and composite types.

### Verify

```bash
# Discover and run the project test suite to ensure all provider schema parsing tests pass against simulated external payloads.
# Discover and run the project type checker and static analysis suite to verify strict type inference from parsed schemas.
```

**Accept when:**
- All integration test suites pass with validated external payload assertions.
- Static type checking confirms all downstream consumers receive validated and inferred data types without manual type casting.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>